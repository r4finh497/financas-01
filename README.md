# 💰 Controle Financeiro Pessoal

## 👨‍💻 Desenvolvido por  
**Rafael Haintz da Silva**

## 📚 Disciplina  
**Desenvolvimento de Aplicações Mobile**

---

## 📱 Sobre o Projeto

Aplicativo mobile completo para **controle de finanças pessoais**, desenvolvido com **React Native** e **Expo**.  
O app permite gerenciar **receitas**, **despesas** e **categorias**, além de visualizar **relatórios financeiros interativos** em uma interface moderna e intuitiva.

---

## 🚀 Funcionalidades

### 🔐 Autenticação Segura
- Cadastro e login de usuários  
- Autenticação via **Supabase Auth**  
- Sessão persistente

### 📊 Dashboard Interativo
- Saldo atual em tempo real  
- Gráficos de **Receitas vs Despesas**  
- Destaques financeiros e visão mensal consolidada

### 💸 Gestão Financeira
- Cadastro de **Despesas** (valor, categoria, data, descrição, comprovante)  
- Cadastro de **Receitas** (valor, fonte, data, descrição)  
- Upload de comprovantes (câmera ou galeria)  
- Validação em tempo real dos dados

### 🗂️ Categorias Personalizáveis
- Categorias padrão criadas automaticamente  
- CRUD completo para categorias personalizadas  
- Organização por tipo (Receita / Despesa)

### 📈 Relatórios e Filtros
- Listagem completa de transações  
- Filtros por **data**, **tipo** e **categoria**  
- Busca textual e ordenação por data

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Função |
|-------------|--------|
| **Expo** | Framework para desenvolvimento mobile |
| **React Native** | Interface nativa multiplataforma |
| **Supabase** | Backend (BaaS) com autenticação e banco de dados |
| **Victory Native** | Criação de gráficos e visualizações |
| **React Navigation** | Navegação entre telas |
| **Expo Image Picker** | Upload e gerenciamento de imagens |

---

## 📸 Telas do Aplicativo

| Login | Dashboard | Receitas |
|:--:|:--:|:--:|
| <img width="300" src="https://github.com/user-attachments/assets/c78c6f4f-a2b5-43bd-891e-53e4cc5be6ad" /> | <img width="300" src="https://github.com/user-attachments/assets/c7357e81-061e-4d41-9d50-6f6f336116c1" /> | <img width="300" src="https://github.com/user-attachments/assets/cf3df12f-79e2-4d33-a169-940cdb4de612" /> |

| Despesas | Categorias | Histórico |
|:--:|:--:|:--:|
| <img width="300" src="https://github.com/user-attachments/assets/f4eb7f4f-7a9d-4c09-b5e5-f9aba7d41efe" /> | <img width="300" src="https://github.com/user-attachments/assets/0e9214a0-9890-4dae-8cd3-de79d8c27e48" /> | <img width="300" src="https://github.com/user-attachments/assets/9f8e4d69-e343-403e-a42e-84b4283a2a89" /> |

| Relatórios | Filtros |
|:--:|:--:|
| <img width="300" src="https://github.com/user-attachments/assets/02e561d2-134e-41b0-aa17-80734bcf6803" /> | <img width="300" src="https://github.com/user-attachments/assets/02e561d2-134e-41b0-aa17-80734bcf6803" /> |

---

## ⚙️ Instalação e Configuração

### 📋 Pré-requisitos
Antes de iniciar, verifique se possui instalado:
- **Node.js** (versão 16 ou superior)
- **npm** ou **yarn**
- **Expo CLI** instalada globalmente
- **Dispositivo móvel com Expo Go** ou **emulador Android/iOS**

### 🧭 Passos de Instalação

```bash
# 1. Clone o repositório
git clone <url-do-repositorio>

# 2. Acesse o diretório do projeto
cd financas-pessoais

# 3. Instale as dependências
npm install
# ou
yarn install

# 4. Inicie o projeto
npx expo start
