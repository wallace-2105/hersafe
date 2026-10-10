<div align="center">

<img src="./src/assets/image.png" alt="HerSafe Logo" width="96" />

# 🛡️ HerSafe

### Life360, só que pensado de verdade para a segurança das mulheres.

Localização em tempo real entre grupos de confiança, botão de pânico (SOS), locais seguros mapeados e uma central de apoio com dicas, autodefesa e canais de denúncia — tudo em um único app.
<img width="591" height="1052" alt="WhatsApp Image 2026-07-21 at 11 00 41" src="https://github.com/user-attachments/assets/9eb2c78f-1ae3-4340-b2b1-df2282ba3c5f" />
<img width="591" height="1050" alt="WhatsApp Image 2026-07-21 at 11 00 41 (1)" src="https://github.com/user-attachments/assets/601b7e4f-258f-443d-bf45-ce6403b01fdd" />




[![React Native](https://img.shields.io/badge/React%20Native-0.81-61DAFB?logo=react&logoColor=black)](https://reactnative.dev/)
[![Expo](https://img.shields.io/badge/Expo-SDK%2054-000020?logo=expo&logoColor=white)](https://expo.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![NativeWind](https://img.shields.io/badge/NativeWind-Tailwind%20CSS-38BDF8?logo=tailwindcss&logoColor=white)](https://www.nativewind.dev/)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](#-licença)

[Visão geral](#-sobre-o-projeto) •
[Funcionalidades](#-funcionalidades) •
[Stack](#-stack-técnica) •
[Arquitetura](#-arquitetura-do-projeto) •
[Como rodar](#-como-rodar-localmente) •
[Design System](#-design-system) •
[Roadmap](#-roadmap)

</div>

---

## 📌 Sobre o projeto

**HerSafe** é um aplicativo mobile de segurança pessoal voltado para o público feminino, construído com **React Native + Expo**. A proposta é simples e direta: dar a uma mulher e à sua rede de confiança (família, amigos, parceiro) uma forma rápida de **saber onde ela está, saber que ela está segura, e agir rápido se algo der errado**.

O app funciona em cima de **grupos de confiança**: você cria ou entra em um grupo, compartilha sua localização com quem importa, cadastra seus locais recorrentes (casa, trabalho, faculdade, academia) e, em caso de emergência, aciona todo o grupo com um único gesto.

Este repositório contém o **app mobile (frontend)**. A API que sustenta autenticação, grupos, convites e notificações está no repositório irmão:
➡️ **[HerSafe Server](https://github.com/wallace-2105/Hersafe_server)** — Node.js + Express + MongoDB.

---

## ✨ Funcionalidades

### 🗺️ Mapa em tempo real (estilo Life360)
- Visualização de todos os membros do grupo em um mapa interativo (`react-native-maps`), com pins de pessoas e de locais seguros.
- Status de presença (segura / offline) calculado a partir da última localização reportada.
- Drawer lateral com a lista de pessoas do grupo, foto/iniciais e último local visto.

### 🆘 Botão de emergência (SOS)
- Slider de emergência com gesto de "arrastar para confirmar" (evita acionamento acidental) e vibração tátil de feedback.
- Ao ser acionado, dispara uma notificação de emergência para **todos os membros do grupo** via API (`/notificacoes/emergencia`).

### 👥 Grupos de confiança
- Criação de grupos, convite de novos membros, aceite/recusa de convites pendentes e visualização detalhada de cada grupo e seus participantes.

### 📍 Locais seguros ("Meus Locais")
- Cadastro de endereços recorrentes — casa, trabalho, faculdade, escola, academia — com geolocalização (`expo-location`), exibidos como pins categorizados no mapa.

### 🔔 Central de notificações
- Contador de não lidas em tempo real no header do app, com tela dedicada para marcar como lida (individual ou em lote).

### 📚 Central de dicas e apoio
- Conteúdo curado de segurança pessoal, vídeo de técnicas básicas de autodefesa, e links diretos para **canais oficiais de denúncia** (delegacia eletrônica) e para o aplicativo oficial de medida protetiva do governo — abertos em navegador in-app para uma experiência fluida.

### 🔐 Autenticação
- Login e cadastro com JWT, sessão persistida localmente (`AsyncStorage`) e bootstrap automático (o app valida o token salvo antes de decidir entre tela de login ou app principal).

### 👤 Perfil
- Edição de dados pessoais, contato de emergência e gerenciamento dos locais seguros cadastrados.

---

## 🧱 Stack técnica

| Camada | Tecnologia |
|---|---|
| Framework | [React Native](https://reactnative.dev/) `0.81` + [Expo](https://expo.dev/) `SDK 54` |
| Linguagem | TypeScript |
| Estilização | [NativeWind](https://www.nativewind.dev/) (Tailwind CSS para React Native) + design tokens em CSS |
| Navegação | [React Navigation](https://reactnavigation.org/) (Bottom Tabs + Native Stack) |
| Mapas & Geo | `react-native-maps`, `expo-location` |
| Ícones | `lucide-react-native`, `@expo/vector-icons` |
| Animações | `react-native-reanimated` + `react-native-worklets` |
| Estado global | React Context API (`AuthContext`, `NotificationContext`) |
| Persistência local | `@react-native-async-storage/async-storage` |
| Navegador in-app | `expo-web-browser` |
| Qualidade | ESLint + Prettier (com plugin Tailwind) |

---

## 🗂️ Arquitetura do projeto

O código-fonte é isolado em `src/`, separando lógica de aplicação dos arquivos de configuração globais — um padrão comum em projetos Expo maduros:

```
hersafe/
├── App.tsx                     # Entry point — envolve o app nos providers (Auth, Notifications)
├── global.css                  # Design tokens via variáveis CSS + diretivas Tailwind
├── tailwind.config.js          # Mapeia variáveis CSS em classes Tailwind
│
└── src/
    ├── screens/                # Telas da aplicação
    │   ├── LoginScreen.tsx / RegisterScreen.tsx
    │   ├── HomeScreen.tsx           # Mapa + SOS + drawer de pessoas
    │   ├── GroupScreen.tsx / GroupDetailScreen.tsx / CreateGroupScreen.tsx
    │   ├── InvitationsScreen.tsx / InviteUserScreen.tsx
    │   ├── NotificationsScreen.tsx
    │   ├── TipsScreen.tsx           # Central de dicas, autodefesa e denúncia
    │   ├── ProfileScreen.tsx
    │   └── SettingsScreen.tsx
    │
    ├── components/
    │   ├── AppHeader.tsx
    │   ├── GrupCard.tsx
    │   ├── Home/                    # MapBackground, EmergencySlider, PeopleDrawer, Pins...
    │   └── Profile/                 # LocationCard
    │
    ├── navigation/
    │   ├── RootNavigator.tsx        # Decide Auth Stack x App Stack (com base no token salvo)
    │   └── AppNavigator.tsx         # Bottom tabs (Home / Grupos / Perfil / Ajustes)
    │
    ├── context/
    │   ├── AuthContext.tsx          # Sessão, token, bootstrap de autenticação
    │   └── NotificationContext.tsx  # Contador de notificações não lidas
    │
    ├── services/                    # Camada de integração com a API HerSafe Server
    │   ├── api.ts                   # fetch wrapper autenticado
    │   ├── userService.ts / groupService.ts
    │   ├── invitationService.ts / notificationService.ts
    │
    ├── types/                       # Tipagens alinhadas ao contrato da API
    ├── theme/colors.ts               # Tokens de cor como objeto TS (para props nativas)
    └── Data/                         # Mocks para desenvolvimento offline
```

> 📄 Documentação arquitetural completa (navegação, design system e convenções) disponível em [`Docs.md`](./Docs.md).

---

## 🎨 Design System

O app usa um design system centralizado, pensado para consistência visual e fácil manutenção:

1. **`global.css`** define os tokens (`:root { --color-primary: ... }`)
2. **`tailwind.config.js`** mapeia cada token para uma classe Tailwind (`bg-primary`, `text-emergency`, etc.)
3. **`src/theme/colors.ts`** exporta os mesmos tokens como objeto TypeScript, para uso em props nativas que não aceitam `className` (`Switch`, `StatusBar`, etc.)

```tsx
// ✅ Via className (NativeWind) — preferido
<View className="bg-surface border border-border rounded-lg p-4">
  <Text className="text-text font-bold">Olá!</Text>
</View>

// ✅ Via colors.ts — para props nativas
import { colors } from '@/theme/colors';
<Switch trackColor={{ false: colors.surface3, true: colors.primaryMuted }} />
```

**Paleta:** tema dark profundo (`#121218`), **violeta** como cor primária de proteção (`#8B5CF6`) e **rosa** reservado exclusivamente para o botão de SOS (`#E91E8C`) — uma escolha intencional para que a ação de emergência seja sempre a mais visível da interface.

---

## 🚀 Como rodar localmente

### Pré-requisitos
- Node.js 18+
- npm
- App **Expo Go** no celular, ou emulador Android/iOS configurado
- A [API HerSafe Server](https://github.com/wallace-2105/Hersafe_server) rodando (local ou remota)

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/wallace-2105/hersafe.git
cd hersafe

# 2. Instale as dependências
npm install

# 3. Configure a URL da API em src/services/api.ts
#    apontando para o endereço do backend (ex: http://SEU_IP:3000/api)

# 4. Inicie o Metro Bundler
npm start

# Ou diretamente na plataforma desejada:
npm run android
npm run ios
npm run web
```

### Scripts disponíveis

| Comando | Descrição |
|---|---|
| `npm start` | Inicia o Expo Dev Server |
| `npm run android` / `npm run ios` / `npm run web` | Inicia em uma plataforma específica |
| `npm run lint` | Roda ESLint + verificação do Prettier |
| `npm run format` | Corrige lint e formata o código automaticamente |
| `npm run prebuild` | Gera os projetos nativos Android/iOS |

---

## 🔗 Repositório da API

Este app consome a API REST do **[HerSafe Server](https://github.com/wallace-2105/Hersafe_server)**, responsável por autenticação (JWT), usuários, grupos, convites e notificações de emergência.

---

## 🗺️ Roadmap

- [ ] Boletim de ocorrência integrado (registro direto no app, hoje via link para portal oficial)
- [ ] Compartilhamento de localização em tempo real via WebSocket/push notifications
- [ ] Histórico de trajetos e zonas de risco
- [ ] Biblioteca própria de vídeos de autodefesa dentro do app
- [ ] Modo "acompanhar meu trajeto" com temporizador de check-in automático

---

## 📄 Licença

Projeto pessoal desenvolvido para fins de portfólio e estudo. Sinta-se à vontade para explorar o código.

---

<div align="center">
Desenvolvido com foco em impacto social real. 💜
</div>
