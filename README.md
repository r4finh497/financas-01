// src/screens/ExpensesScreen.js
import React, { useState, useEffect } from 'react';
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  StyleSheet,
  Alert,
  ScrollView,
  Image,
} from 'react-native';
import DateTimePicker from '@react-native-community/datetimepicker';
import * as ImagePicker from 'expo-image-picker';
import { useAuth } from '../contexts/AuthContext';
import { supabase } from '../config/supabase';

export default function ExpensesScreen({ navigation }) {
  const [amount, setAmount] = useState('');
  const [description, setDescription] = useState('');
  const [selectedCategoryId, setSelectedCategoryId] = useState(null);
  const [selectedCategoryName, setSelectedCategoryName] = useState('');
  const [date, setDate] = useState(new Date());
  const [showDatePicker, setShowDatePicker] = useState(false);
  const [receipt, setReceipt] = useState(null);
  const [categories, setCategories] = useState([]);
  const [loading, setLoading] = useState(false);
  const { user } = useAuth();

  // CATEGORIAS PADRÃO COMO FALLBACK
  const defaultCategories = [
    'Alimentação', 'Transporte', 'Moradia', 'Saúde',
    'Educação', 'Lazer', 'Compras', 'Outros'
  ];

  useEffect(() => {
    loadCategories();
  }, []);

  const loadCategories = async () => {
    try {
      const { data, error } = await supabase
        .from('categories')
        .select('*')
        .eq('user_id', user.id)
        .eq('type', 'expense');

      if (error) {
        console.log('Erro ao carregar categorias, usando padrão:', error);
        // fallback: criar objetos com id = null e name = string
        setCategories(defaultCategories.map(name => ({ id: null, name })));
      } else {
        if (!data || data.length === 0) {
          setCategories(defaultCategories.map(name => ({ id: null, name })));
        } else {
          setCategories(data.map(c => ({ id: c.id, name: c.name })));
        }
      }
    } catch (error) {
      console.log('Erro crítico, usando categorias padrão:', error);
      setCategories(defaultCategories.map(name => ({ id: null, name })));
    }
  };

  const pickImage = async () => {
    const { status } = await ImagePicker.requestMediaLibraryPermissionsAsync();
    if (status !== 'granted') {
      Alert.alert('Permissão necessária', 'Precisamos de acesso à galeria para anexar comprovantes.');
      return;
    }
    const result = await ImagePicker.launchImageLibraryAsync({
      mediaTypes: ImagePicker.MediaTypeOptions.Images,
      allowsEditing: true,
      aspect: [4, 3],
      quality: 0.8,
    });
    if (!result.canceled) {
      setReceipt(result.assets[0].uri);
    }
  };

  const takePhoto = async () => {
    const { status } = await ImagePicker.requestCameraPermissionsAsync();
    if (status !== 'granted') {
      Alert.alert('Permissão necessária', 'Precisamos de acesso à câmera para tirar fotos.');
      return;
    }
    const result = await ImagePicker.launchCameraAsync({
      allowsEditing: true,
      aspect: [4, 3],
      quality: 0.8,
    });
    if (!result.canceled) {
      setReceipt(result.assets[0].uri);
    }
  };

  const handleSubmit = async () => {
    if (!amount || !description || (!selectedCategoryId && !selectedCategoryName)) {
      Alert.alert('Erro', 'Por favor, preencha valor, descrição e categoria');
      return;
    }

    const amountValue = parseFloat(amount.replace(',', '.'));
    if (isNaN(amountValue) || amountValue <= 0) {
      Alert.alert('Erro', 'Por favor, insira um valor válido maior que zero');
      return;
    }

    setLoading(true);
    try {
      const insertObj = {
        user_id: user.id,
        type: 'expense',
        amount: amountValue,
        description,
        date: date.toISOString().split('T')[0],
        receipt_url: receipt || null,
      };

      // Se categoria tem id (uuid), envia category_id
      if (selectedCategoryId) insertObj.category_id = selectedCategoryId;
      // Sempre envia também o nome da categoria como fallback/texto
      if (selectedCategoryName) insertObj.category = selectedCategoryName;

      const { error } = await supabase
        .from('transactions')
        .insert([insertObj]);

      if (error) throw error;

      Alert.alert('Sucesso', 'Despesa cadastrada com sucesso!');
      navigation.goBack();

    } catch (error) {
      Alert.alert('Erro', 'Não foi possível cadastrar a despesa');
      console.error(error);
    } finally {
      setLoading(false);
    }
  };

  const handleAmountChange = (text) => {
    const cleanedText = text.replace(/[^0-9,.]/g, '');
    // mantemos vírgula no campo para UX local, mas convertemos ao salvar
    setAmount(cleanedText);
  };

  return (
    <ScrollView style={styles.container}>
      <View style={styles.header}>
        <Text style={styles.title}>Nova Despesa</Text>
      </View>

      <View style={styles.form}>
        <TextInput
          style={styles.input}
          placeholder="Valor (R$)"
          value={amount}
          onChangeText={handleAmountChange}
          keyboardType="decimal-pad"
        />

        <TextInput
          style={styles.input}
          placeholder="Descrição"
          value={description}
          onChangeText={setDescription}
        />

        <TouchableOpacity
          style={styles.input}
          onPress={() => setShowDatePicker(true)}
        >
          <Text style={styles.dateText}>
            Data: {date.toLocaleDateString('pt-BR')}
          </Text>
        </TouchableOpacity>

        {showDatePicker && (
          <DateTimePicker
            value={date}
            mode="date"
            display="default"
            onChange={(event, selectedDate) => {
              setShowDatePicker(false);
              if (selectedDate) setDate(selectedDate);
            }}
          />
        )}

        <View style={styles.categoryContainer}>
          <Text style={styles.label}>Categoria:</Text>
          <ScrollView horizontal showsHorizontalScrollIndicator={false} style={styles.categoriesScroll}>
            {categories.map(cat => (
              <TouchableOpacity
                key={cat.id ?? cat.name}
                style={[
                  styles.categoryButton,
                  (selectedCategoryId === cat.id || selectedCategoryName === cat.name) && styles.categoryButtonSelected,
                ]}
                onPress={() => {
                  setSelectedCategoryId(cat.id || null);
                  setSelectedCategoryName(cat.name);
                }}
              >
                <Text style={[
                  styles.categoryText,
                  (selectedCategoryId === cat.id || selectedCategoryName === cat.name) && styles.categoryTextSelected,
                ]}>
                  {cat.name}
                </Text>
              </TouchableOpacity>
            ))}
          </ScrollView>

          {selectedCategoryName ? (
            <Text style={styles.selectedCategory}>
              Categoria selecionada: <Text style={styles.selectedCategoryName}>{selectedCategoryName}</Text>
            </Text>
          ) : (
            <Text style={styles.noCategorySelected}>Nenhuma categoria selecionada</Text>
          )}
        </View>

        <View style={styles.receiptSection}>
          <Text style={styles.label}>Comprovante (opcional):</Text>
          {receipt && <Image source={{ uri: receipt }} style={styles.receiptImage} />}
          <View style={styles.receiptButtons}>
            <TouchableOpacity style={styles.receiptButton} onPress={pickImage}>
              <Text style={styles.receiptButtonText}>📁 Galeria</Text>
            </TouchableOpacity>
            <TouchableOpacity style={styles.receiptButton} onPress={takePhoto}>
              <Text style={styles.receiptButtonText}>📷 Câmera</Text>
            </TouchableOpacity>
            {receipt && (
              <TouchableOpacity style={[styles.receiptButton, styles.removeButton]} onPress={() => setReceipt(null)}>
                <Text style={styles.receiptButtonText}>❌ Remover</Text>
              </TouchableOpacity>
            )}
          </View>
        </View>

        <TouchableOpacity
          style={[styles.submitButton, loading && styles.submitButtonDisabled]}
          onPress={handleSubmit}
          disabled={loading}
        >
          <Text style={styles.submitButtonText}>
            {loading ? 'Cadastrando...' : 'Cadastrar Despesa'}
          </Text>
        </TouchableOpacity>
      </View>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  /* ========== Mantive os mesmos estilos do seu arquivo original ========== */
  container: { flex: 1, backgroundColor: '#f5f5f5' },
  header: { backgroundColor: 'white', padding: 20, paddingTop: 60 },
  title: { fontSize: 24, fontWeight: 'bold', color: '#333' },
  form: { padding: 20 },
  input: { backgroundColor: 'white', padding: 15, borderRadius: 10, marginBottom: 15, borderWidth: 1, borderColor: '#ddd', fontSize: 16 },
  dateText: { fontSize: 16, color: '#333' },
  label: { fontSize: 16, fontWeight: 'bold', marginBottom: 5, color: '#333' },
  categoryContainer: { marginBottom: 20, backgroundColor: 'white', padding: 15, borderRadius: 10, borderWidth: 1, borderColor: '#ddd' },
  categoriesScroll: { maxHeight: 60, marginBottom: 10 },
  categoryButton: { backgroundColor: '#f8f9fa', paddingHorizontal: 15, paddingVertical: 10, borderRadius: 20, marginRight: 10, borderWidth: 1, borderColor: '#dee2e6' },
  categoryButtonSelected: { backgrou
