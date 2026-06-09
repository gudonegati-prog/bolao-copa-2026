# ⚽ Bolão Copa do Mundo 2026

> Sistema web colaborativo para bolão da Copa do Mundo 2026, com sincronização em nuvem e classificação ao vivo.

![Status](https://img.shields.io/badge/status-em%20produção-success)
![License](https://img.shields.io/badge/license-MIT-blue)
![Tech](https://img.shields.io/badge/stack-HTML%20%7C%20JS%20%7C%20Supabase-orange)

---

## 📖 A história do projeto

Tudo começou com um pedido simples: organizar um bolão para a galera do trabalho acompanhar a Copa do Mundo 2026. A primeira ideia foi uma **planilha em Excel** — modelo para os participantes preencherem e uma central de controle.

Mas logo veio a pergunta: *"e se em vez de mandar arquivo Excel para cada um, fizéssemos algo automatizado?"*

Daí nasceu este projeto. A jornada passou por várias arquiteturas até chegar à solução final:

| Versão | Arquitetura | Por que mudou |
|---|---|---|
| v1 | Planilhas Excel | Trabalhoso receber 20+ arquivos de volta |
| v2 | HTML com `localStorage` | Dados ficavam só no navegador de cada pessoa |
| v3 | HTML + JSONBin.io | Cada navegador criava um "bin" diferente, sem sincronização real |
| v4 | HTML + **Supabase** | ✅ Sincronização real, multi-usuário, gratuito |

---

## 🎯 Funcionalidades

- 🏆 **72 jogos da Fase de Grupos** com datas, horários (Brasília) e sedes oficiais
- 🥇 **Eliminatórias progressivas** — Oitavas, Quartas, Semis e Final liberadas pelo organizador conforme os times se classificam
- 👤 **Autenticação por PIN pessoal** — cada participante cria sua própria senha
- ✅ **Aprovação manual** pelo organizador (após confirmação de pagamento)
- ⏰ **Trava automática** de palpites 1 hora antes do início de cada jogo
- 🔒 **Trava do palpite de Campeão** no início da Copa
- 📊 **Classificação ao vivo** atualizada a cada 30 segundos
- 🔍 **Aba de detalhes** com comparativo palpite × resultado real, jogo a jogo
- 📋 **Aba "Palpites de Todos"** (somente leitura) para consulta após aprovação
- 💰 **Tela de pagamento** com QR Code Pix gerado dinamicamente
- 🏳️ **Bandeiras dos países** em todos os confrontos
- ☁️ **Sincronização em nuvem** com fallback offline (localStorage)

---

## 🛠️ Stack técnica

- **Frontend:** HTML5 + CSS3 + JavaScript vanilla (sem framework)
- **Backend:** [Supabase](https://supabase.com) (PostgreSQL gerenciado + API REST)
- **Hospedagem:** [Netlify](https://netlify.com) (drag & drop)
- **Tipografia:** Google Fonts (Bebas Neue, DM Sans, JetBrains Mono)
- **QR Code Pix:** API pública do QRServer + payload Pix gerado em JS

> Decisão arquitetural: optei por **JS puro sem framework** para manter o arquivo único, fácil de distribuir, sem build step e leve para abrir em qualquer dispositivo.

---

## 🚀 Como instalar para o seu bolão

### 1️⃣ Criar conta gratuita no Supabase

1. Acesse [supabase.com](https://supabase.com) e crie uma conta
2. Crie um novo projeto (escolha a região mais próxima)
3. Aguarde a inicialização (~1 minuto)

### 2️⃣ Criar a tabela do banco

No painel do Supabase, vá em **SQL Editor** e execute:

```sql
CREATE TABLE bolao (
  id TEXT PRIMARY KEY,
  data JSONB NOT NULL DEFAULT '{}'::jsonb,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

INSERT INTO bolao (id, data) VALUES ('copa2026', '{}'::jsonb);

ALTER TABLE bolao ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon_read" ON bolao FOR SELECT TO anon USING (true);
CREATE POLICY "anon_update" ON bolao FOR UPDATE TO anon USING (true) WITH CHECK (true);

GRANT SELECT, UPDATE ON bolao TO anon;
```

### 3️⃣ Pegar suas credenciais

Em **Project Settings → API Keys → Legacy anon, service_r...**, copie:
- A **Project URL** (formato `https://xxxxx.supabase.co`)
- A chave **anon / public** (começa com `eyJ...`)

### 4️⃣ Editar o arquivo HTML

Abra `bolao_copa2026.html` em qualquer editor de texto e localize:

```javascript
const SB_URL = "https://SEU-PROJETO.supabase.co";
const SB_KEY = "SUA_CHAVE_ANON_DO_SUPABASE";
const PIX_KEY = "seu-email@exemplo.com";
const PIX_NAME = "SEU NOME";
```

Substitua pelos seus valores.

### 5️⃣ Publicar online

Acesse [drop.netlify.com](https://drop.netlify.com) e arraste o arquivo HTML. Pronto, você terá um link público para compartilhar.

---

## ⚙️ Configurações do organizador

- **Senha padrão:** `copa2026` (alterar no primeiro acesso!)
- **Pontuação configurada:**
  - Fase de Grupos: 3 pts (exato) / 1 pt (resultado certo)
  - Oitavas: 6 pts / 3 pts
  - Quartas: 8 pts / 4 pts
  - Semis: 10 pts / 5 pts
  - Final: 15 pts / 7 pts
  - 🏆 Bônus campeão: +20 pts

---

## 📸 Screenshots

> *Adicione aqui prints do app: tela inicial, palpites, classificação, painel do organizador.*

---

## 🎓 O que aprendi neste projeto

- Migrar de armazenamento local para nuvem sem quebrar dados já cadastrados
- Trabalhar com Row Level Security (RLS) no PostgreSQL via Supabase
- Lidar com fuso horário e datas em JavaScript (horários FIFA → Brasília)
- Gerar payloads de Pix com CRC16 em JS puro
- Resolver bugs de produção preservando os dados dos usuários
- UX: diferença entre travar dado (`disabled`) vs comunicar trava (countdown visual)
- Distribuição zero-friction: um arquivo HTML único, sem build, sem deploy complexo

---

## 📝 Licença

MIT — sinta-se livre para adaptar e usar no seu próprio bolão!

---

**Feito com ⚽ por [Gustavo Donegati](https://github.com/gudonegati-prog)**
