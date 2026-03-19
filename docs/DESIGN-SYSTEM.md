# JWL TECH — Design System & Guia de Padronização

Documento de referência para criação e manutenção de todas as páginas do site artjason.com.
Toda nova página ou feature deve seguir este guia sem exceções.

---

## Índice

1. [Estrutura de Arquivos](#1-estrutura-de-arquivos)
2. [Cores](#2-cores)
3. [Tipografia](#3-tipografia)
4. [Espaçamento & Layout](#4-espaçamento--layout)
5. [Componentes](#5-componentes)
6. [Animações](#6-animações)
7. [Responsividade](#7-responsividade)
8. [Tom de Voz](#8-tom-de-voz)
9. [Criando uma Nova Página](#9-criando-uma-nova-página)
10. [Checklist de Revisão](#10-checklist-de-revisão)

---

## 1. Estrutura de Arquivos

```
infra/artjason-site/
├── index.html          ← Home (página principal)
├── sobre.html
├── servicos.html
├── projetos.html
├── cases.html
├── stack.html
├── blog.html
├── DESIGN-SYSTEM.md    ← este arquivo
└── base/               ← manifests Kubernetes (não editar manualmente)
```

**Regras:**
- Cada página é um arquivo `.html` independente, sem frameworks ou bundlers
- CSS e JS ficam embutidos no próprio HTML (sem arquivos externos separados)
- Sem dependências de npm, webpack, vite ou similares — o projeto é HTML puro
- A única dependência externa permitida é o Google Fonts (já configurado)

---

## 2. Cores

Todas as cores são definidas via CSS custom properties no `:root`. **Nunca use valores hexadecimais diretamente no CSS** — sempre referencie a variável.

```css
:root {
    --bg-primary:      #0a0a0f;   /* fundo principal — preto azulado profundo */
    --bg-secondary:    #0f0f18;   /* fundo de seções alternadas */
    --bg-card:         #12121e;   /* fundo de cards e elementos elevados */
    --bg-card-hover:   #181828;   /* fundo de cards no hover */

    --text-primary:    #e8e8ef;   /* texto principal */
    --text-secondary:  #7a7a8e;   /* texto de apoio, descrições */
    --text-muted:      #4a4a5e;   /* texto terciário, labels, placeholders */

    --accent:          #00e5a0;   /* verde principal — cor de destaque */
    --accent-dim:      rgba(0, 229, 160, 0.08);   /* fundo sutil com acento */
    --accent-glow:     rgba(0, 229, 160, 0.15);   /* glow/sombra do acento */
    --accent-secondary:#00b8d4;   /* azul — acento secundário (uso esporádico) */

    --border:          rgba(255, 255, 255, 0.06);  /* borda padrão */
    --border-hover:    rgba(0, 229, 160, 0.2);     /* borda no hover */
}
```

### Uso das cores por contexto

| Contexto | Variável |
|---|---|
| Fundo da página | `--bg-primary` |
| Seções alternadas (sobre, cases) | `--bg-secondary` |
| Cards, terminais, inputs | `--bg-card` |
| Hover de cards | `--bg-card-hover` |
| Títulos e texto principal | `--text-primary` |
| Parágrafos e descrições | `--text-secondary` |
| Labels, metadados, placeholders | `--text-muted` |
| Destaques, ícones, números, links ativos | `--accent` |
| Fundos sutis com destaque | `--accent-dim` |
| Box-shadow e glows | `--accent-glow` |
| Bordas padrão | `--border` |
| Bordas em hover | `--border-hover` |

---

## 3. Tipografia

### Fontes

```css
--font-display: 'Outfit', sans-serif;       /* corpo, títulos, UI geral */
--font-mono:    'JetBrains Mono', monospace; /* labels, código, navbar, botões */
```

Import no `<head>` de cada página:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

### Escala de tamanhos

| Elemento | Fonte | Tamanho | Peso | Observação |
|---|---|---|---|---|
| Hero title | Outfit | `clamp(2.8rem, 6vw, 4.8rem)` | 700 | `letter-spacing: -0.03em` |
| Section title | Outfit | `clamp(1.8rem, 3.5vw, 2.6rem)` | 600 | `letter-spacing: -0.02em` |
| Card title (h3) | Outfit | `1.15rem` | 600 | `letter-spacing: -0.01em` |
| Card title (h4) | Outfit | `1rem` | 600 | — |
| Parágrafo / desc | Outfit | `1rem` | 300 | `line-height: 1.7` |
| Parágrafo card | Outfit | `0.92rem` | 300 | `line-height: 1.65` |
| Section label | JetBrains Mono | `0.72rem` | 400 | uppercase, `letter-spacing: 0.14em` |
| Navbar links | JetBrains Mono | `0.74rem` | 400 | uppercase, `letter-spacing: 0.08em` |
| Botões | JetBrains Mono | `0.82rem` | 600 | `letter-spacing: 0.04em` |
| Form label | JetBrains Mono | `0.72rem` | 400 | uppercase, `letter-spacing: 0.08em` |
| Footer texto | JetBrains Mono | `0.72rem` | 400 | `letter-spacing: 0.04em` |
| Números de destaque | JetBrains Mono | `2rem`–`2.8rem` | 700 | `letter-spacing: -0.03em` |

### Regra geral

- **Outfit** → tudo que é leitura: títulos, parágrafos, descrições
- **JetBrains Mono** → tudo que é interface/metadata: labels, botões, navbar, rodapé, números, código

---

## 4. Espaçamento & Layout

### Container

```css
.container {
    max-width: 1140px;
    margin: 0 auto;
    padding: 0 24px;
}
```

### Padding de seções

```css
section {
    padding: 120px 0;        /* desktop */
}

@media (max-width: 640px) {
    section { padding: 80px 0; }
}
```

### Grids padrão

| Uso | Columns | Gap |
|---|---|---|
| 2 colunas (texto + visual) | `1fr 1fr` ou `1fr 1.2fr` | `80px` |
| 3 colunas (cards de serviço) | `repeat(3, 1fr)` | `20px` |
| 4 colunas (números/stats) | `repeat(4, 1fr)` | `2px` |
| Stats 2x2 | `1fr 1fr` | `24px` |
| Footer | `1.5fr 1fr 1fr` | `48px` |

### Espaçamentos internos de cards

```css
/* card padrão */
padding: 40px 32px;

/* card compacto (segment, contact item) */
padding: 32px;

/* stat card */
padding: 24px;
```

---

## 5. Componentes

### Navbar

Presente em **todas** as páginas. Copiar exatamente do `index.html`.

- Logo: `JWL TECH` com `logo-dot` pulsante
- Links externos: `href="pagina.html"`
- Âncoras (apenas na home): `href="#section-id"`
- Em páginas internas, âncoras da home devem apontar para `index.html#section-id`
- "Contato" sempre com classe `nav-cta` (borda verde)
- JS de scroll e menu mobile sempre incluído

### Section Label

```html
<p class="section-label">Texto do label</p>
```

Sempre maiúsculo, com linha verde à esquerda. Em headers centralizados, a linha aparece dos dois lados (via `::before` removido e `::after` adicionado).

### Section Header centralizado

```html
<div class="services-header reveal">
    <p class="section-label">Label</p>
    <h2 class="section-title">Título da seção</h2>
    <p class="section-desc">Descrição curta da seção.</p>
</div>
```

CSS extra necessário quando centralizado:

```css
.nome-header { text-align: center; margin-bottom: 64px; }
.nome-header .section-label { justify-content: center; }
.nome-header .section-label::before { display: none; }
.nome-header .section-label::after { content: ''; width: 24px; height: 1px; background: var(--accent); }
.nome-header .section-desc { margin: 0 auto; }
```

### Card padrão (service-card)

```html
<div class="service-card reveal">
    <div class="service-icon">01</div>
    <h3>Título do card</h3>
    <p>Descrição do card.</p>
</div>
```

- `service-icon`: numeração sequencial `01`, `02`... ou tag curta em mono
- Hover: `translateY(-4px)` + borda verde + linha superior com gradiente

### Stat card

```html
<div class="stat-card">
    <div class="stat-number">99.9%</div>
    <div class="stat-label">Uptime alvo</div>
</div>
```

### Segment card (com ícone SVG)

```html
<div class="segment-card reveal">
    <div class="segment-icon">
        <!-- SVG 20x20, stroke="currentColor", stroke-width="1.5" -->
    </div>
    <div class="segment-content">
        <h4>Título</h4>
        <p>Descrição curta.</p>
    </div>
</div>
```

### Botões

```html
<!-- Primário: fundo verde, texto escuro -->
<a href="#" class="btn btn-primary">
    Texto
    <svg><!-- seta --></svg>
</a>

<!-- Ghost: transparente, borda sutil -->
<a href="#" class="btn btn-ghost">Texto</a>
```

### Terminal window

```html
<div class="terminal-window">
    <div class="terminal-header">
        <div class="terminal-dot"></div>
        <div class="terminal-dot"></div>
        <div class="terminal-dot"></div>
    </div>
    <div class="terminal-body">
        <div><span class="prompt">user@host</span> <span class="cmd">~ $ comando</span></div>
        <div class="output">▸ saída do comando</div>
        <div><span class="prompt">user@host</span> <span class="cmd">~ $ </span><span class="terminal-cursor"></span></div>
    </div>
</div>
```

### Ícones SVG

- Sempre inline (sem bibliotecas externas)
- Atributos padrão: `width="N" height="N" viewBox="0 0 24 24" fill="none" stroke="currentColor"`
- `stroke-width`: `1.5` em ícones decorativos, `2` em ícones de botão/ação
- Cor herdada via `currentColor` — nunca hardcode de cor no SVG

### Formulário

```html
<div class="form-group">
    <label class="form-label" for="campo">Label</label>
    <input type="text" id="campo" class="form-input" placeholder="...">
</div>

<div class="form-group">
    <label class="form-label" for="msg">Mensagem</label>
    <textarea id="msg" class="form-textarea" placeholder="..."></textarea>
</div>
```

### Footer

Presente em **todas** as páginas. Estrutura com 3 colunas:
1. Brand (logo + descrição curta)
2. Navegação (todos os links)
3. Contato rápido (email, telefone, formulário)

Footer bottom com copyright e links rápidos.

---

## 6. Animações

### Fade up (entrada de hero)

```css
@keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
}
```

Aplicar com delay crescente nos elementos do hero: `0.2s`, `0.4s`, `0.6s`, `0.8s`.

### Scroll reveal

Adicionar classe `reveal` a qualquer elemento que deva aparecer ao rolar:

```html
<div class="reveal">...</div>
```

O JS do `IntersectionObserver` cuida do resto. **Não remover o JS de reveal de nenhuma página.**

```css
.reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.7s var(--ease-out);
}
.reveal.visible {
    opacity: 1;
    transform: translateY(0);
}
```

### Ease padrão

```css
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);
```

Usar em todas as transições que precisam de "spring" suave. Transições simples de cor usam `ease` padrão.

### Pulse do logo-dot

```css
@keyframes pulse-dot {
    0%, 100% { opacity: 1; box-shadow: 0 0 12px var(--accent-glow); }
    50%       { opacity: 0.6; box-shadow: 0 0 4px var(--accent-glow); }
}
```

### Cursor piscante (terminal)

```css
@keyframes blink { 50% { opacity: 0; } }
```

---

## 7. Responsividade

### Breakpoints

| Breakpoint | Uso |
|---|---|
| `max-width: 1100px` | Ajuste fino de navbar (gap menor) |
| `max-width: 968px` | Tablet: grids viram 1–2 colunas |
| `max-width: 640px` | Mobile: navbar colapsa, tudo em 1 coluna |

### Comportamento por breakpoint

**968px:**
- Grid 2 colunas (texto + visual) → 1 coluna
- Grid 3 colunas (cards) → 2 colunas
- Grid 4 colunas (números) → 2 colunas
- Footer 3 colunas → 2 colunas, brand ocupa linha inteira

**640px:**
- `nav-links` some, `menu-toggle` aparece
- Todas as grids → 1 coluna
- `form-row` → 1 coluna
- `hero-actions` → coluna (botões empilhados)
- Footer → 1 coluna, footer-bottom empilhado

### Menu mobile

```css
.nav-links.active {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 100%;
    left: 0; right: 0;
    background: rgba(10, 10, 15, 0.97);
    backdrop-filter: blur(20px);
    padding: 24px;
    gap: 20px;
    border-bottom: 1px solid var(--border);
}
```

---

## 8. Tom de Voz

O site representa uma **consultoria de TI orientada a solução de negócio**, não um produto de software.

### Princípios

- **Direto:** frases curtas, sem jargão desnecessário
- **Confiante:** afirmações, não promessas vagas
- **Técnico mas acessível:** usar termos técnicos quando necessário, mas sempre com contexto
- **Orientado a resultado:** falar do impacto no negócio, não só da tecnologia

### Exemplos de tom

| Evitar | Preferir |
|---|---|
| "Oferecemos soluções inovadoras de TI" | "Sua operação funcionando sem imprevistos" |
| "Nossa plataforma de monitoramento" | "Visibilidade completa da sua infraestrutura" |
| "Utilizamos tecnologias de ponta" | "Stack comprovada em ambiente de produção" |
| "Fale com nossa equipe" | "Conte o que precisa. Sem burocracia." |

### Seções e seus focos

| Seção | Foco do texto |
|---|---|
| Hero | Proposta de valor central — o que a empresa resolve |
| Sobre | Quem somos, como operamos, diferenciais de processo |
| Serviços | O que fazemos e o resultado que entregamos |
| Cases | Prova social — números, segmentos, resultados reais |
| Projetos | Trabalhos realizados com contexto e resultado |
| Stack | Arsenal técnico — tecnologias que usamos e por quê |
| Blog | Conteúdo de autoridade — educação e posicionamento |
| Contato | CTA direto — zero fricção |

---

## 9. Criando uma Nova Página

### Template base

Toda nova página começa com esta estrutura:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOME DA PÁGINA — JWL TECH</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* Cole aqui o bloco completo de RESET & VARIABLES do index.html */
        /* Cole aqui o CSS do NAVBAR */
        /* Cole aqui o CSS do FOOTER */
        /* Cole aqui o CSS de SCROLL ANIMATIONS */
        /* Cole aqui o CSS de RESPONSIVE */
        /* Adicione abaixo o CSS específico desta página */
    </style>
</head>
<body>

    <!-- Navbar: copiar exatamente do index.html -->
    <!-- Marcar o link da página atual com aria-current="page" se quiser -->

    <!-- Hero da página (opcional) -->
    <section class="page-hero">
        <div class="hero-grid-bg"></div>
        <div class="container">
            <div class="page-hero-content">
                <p class="section-label">Label da seção</p>
                <h1 class="section-title">Título da Página</h1>
                <p class="section-desc">Descrição curta.</p>
            </div>
        </div>
    </section>

    <!-- Conteúdo específico da página -->

    <!-- Footer: copiar exatamente do index.html -->

    <script>
        /* Cole aqui o JS do navbar scroll, menu mobile e scroll reveal */
        /* Adicione abaixo o JS específico desta página */
    </script>
</body>
</html>
```

### Passos para criar uma nova página

1. Copie o `index.html` como ponto de partida
2. Altere o `<title>` para `NOME — JWL TECH`
3. Remova as seções que não se aplicam (Hero, Sobre, etc.)
4. Adicione o hero da página interna (mais compacto que o da home)
5. Construa o conteúdo usando os componentes deste guia
6. Mantenha o navbar e footer intactos
7. Verifique o checklist abaixo antes de subir

### Hero de página interna (padrão)

Mais compacto que o da home — sem `min-height: 100vh`:

```css
.page-hero {
    padding: 160px 0 80px;  /* espaço extra no topo por causa do navbar fixo */
    position: relative;
    border-bottom: 1px solid var(--border);
}
```

---

## 10. Checklist de Revisão

Antes de fazer deploy de qualquer página ou alteração:

### Visual
- [ ] Todas as cores usam variáveis CSS (nenhum hex hardcoded no CSS da página)
- [ ] Fontes corretas: Outfit para texto, JetBrains Mono para UI/labels/botões
- [ ] Navbar idêntico ao do `index.html`
- [ ] Footer idêntico ao do `index.html`
- [ ] Noise overlay (`body::before`) presente
- [ ] Logo-dot pulsante presente e funcional

### Componentes
- [ ] Todos os cards usam a classe `reveal`
- [ ] SVGs sem cor hardcoded (apenas `currentColor`)
- [ ] Botões usando classes `.btn` + `.btn-primary` ou `.btn-ghost`
- [ ] Section labels com a linha verde à esquerda (ou centralizada quando aplicável)

### Responsividade
- [ ] Testado em 1440px (desktop)
- [ ] Testado em 768px (tablet)
- [ ] Testado em 375px (mobile)
- [ ] Menu mobile abre e fecha corretamente
- [ ] Nenhum elemento com overflow horizontal no mobile

### Conteúdo
- [ ] Tom de voz orientado a solução, não a software
- [ ] Nenhum texto placeholder esquecido
- [ ] Links do navbar apontando para os arquivos corretos
- [ ] Links âncoras de páginas internas apontando para `index.html#ancora`

### Técnico
- [ ] Sem dependências externas novas além do Google Fonts
- [ ] JS de scroll reveal presente e funcionando
- [ ] JS de navbar scroll presente
- [ ] JS de menu mobile presente
- [ ] Sem erros no console do browser

---

*Última atualização: 2026-03-18*
