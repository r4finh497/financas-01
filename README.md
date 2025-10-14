# 💰 Controle Financeiro Pessoal

## 👨‍💻 Desenvolvido por
**Rafael Haintz da Silva**

## 📚 Disciplina
**Desenvolvimento de Aplicações Mobile**

---

## 📱 Sobre o Projeto
Aplicativo mobile completo para **controle de finanças pessoais**, desenvolvido com **React Native** e **Expo**.  
Permite gerenciar **receitas**, **despesas** e **categorias**, além de visualizar **relatórios financeiros interativos** em uma interface moderna e intuitiva.

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
- **Expo** – Framework de desenvolvimento mobile  
- **React Native** – Interface nativa  
- **Supabase** – Backend (BaaS)  
- **Victory Native** – Gráficos e visualizações  
- **React Navigation** – Navegação entre telas  
- **Expo Image Picker** – Upload de imagens  

---

## 📸 Telas do Aplicativo
- **Tela de Login** – Autenticação limpa e direta  
- **Tela de Cadastro** – Criação de conta com validação  
- **Dashboard** – Visão geral com gráficos e métricas  
- **Cadastro de Receitas** – Registro de entradas financeiras  
- **Cadastro de Despesas** – Registro detalhado de gastos  
- **Gestão de Categorias** – CRUD completo de categorias  
- **Lista de Transações** – Histórico filtrado de movimentações  

---

## ⚙️ Instalação e Configuração

### 📋 Pré-requisitos
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
