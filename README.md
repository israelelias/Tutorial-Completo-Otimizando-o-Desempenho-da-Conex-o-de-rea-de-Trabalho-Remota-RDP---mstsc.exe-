# 🧠 **Tutorial Completo — Otimizando o Desempenho da Conexão de Área de Trabalho Remota (RDP - mstsc.exe)**

> Ideal para quem acessa o ambiente de trabalho via RDP e quer reduzir **delay, travamentos e lentidão**.

---

## ⚙️ **1. Antes de começar**

### ✅ Pré-requisitos:

- Conexão por **cabo de rede (Ethernet)** — de preferência **diretamente**, mas se usar **adaptador USB para RJ45**, também funciona bem.

- Wi-Fi **desativado** (para evitar interferência).

- Sistema operacional Windows (cliente local).

- Acesso remoto via **mstsc.exe** (Cliente RDP padrão da Microsoft).

---

## 🧩 **2. Otimizações no seu computador (fora da área remota)**

Estas configurações são feitas **no seu computador local**, antes de abrir o ambiente remoto.

---

### 🧱 **2.1 Desativar o Wi-Fi (para evitar conflito com o cabo)**

1. Abra o **Prompt de Comando como Administrador**
   
   - Pressione `Win + S`, digite **cmd** → clique com o botão direito → **Executar como administrador**

2. Execute o comando abaixo:
   
   ```bash
   netsh interface set interface "Wi-Fi" admin=disabled
   ```
   
   🔹 Isso garante que o sistema use **somente o cabo de rede (Ethernet)**.

3. Caso queira reativar depois:
   
   ```bash
   netsh interface set interface "Wi-Fi" admin=enabled
   ```

---

### 🧭 **2.2 Ajustar as configurações de desempenho no mstsc.exe**

1. Pressione `Win + R` → digite:
   
   ```
   mstsc
   ```
   
   e pressione **Enter**.

2. Clique em **Mostrar opções ▾**

3. Configure as guias da seguinte forma:

#### 🧩 Guia **Exibição**

- **Resolução:** escolha **“Usar todos os meus monitores”** se tiver dois monitores.  
  (Ou marque **"Tela cheia"** para um só.)

- **Profundidade de cor:** selecione **16 bits (Alta cor)** — mais leve e suficiente.

#### 🎨 Guia **Recursos Locais**

- Clique em **Mais...**
  
  - **Desmarque**: Impressoras, Áudio, Portas e Unidades de disco.
  
  - **Mantenha marcado** apenas o que for **estritamente necessário** (como área de transferência, se precisar copiar e colar texto).

#### 🚀 Guia **Desempenho**

- Em “Velocidade da sua conexão”, selecione **“Baixa velocidade de banda larga (2 Mbps - 10 Mbps)”**  
  → Isso faz o RDP desativar automaticamente **animações, temas e transparências**.

- **Marque**:
  
  - “Detectar automaticamente a velocidade da conexão”
  
  - “Persistir bitmaps em cache” ✅

---

### 🔧 Seção 2.3 — **Ajustes no adaptador de rede USB (fora da área remota)**

> 🔹 Essas configurações são feitas **no seu computador local**  
> 🔹 Melhoram a estabilidade da conexão e reduzem microdesconexões no adaptador de rede USB

### Passos:

1. Pressione `Win + X` → **Gerenciador de Dispositivos**

2. Expanda **Adaptadores de rede**

3. Clique duas vezes sobre o seu **adaptador USB Ethernet**

4. Vá até a aba **Gerenciamento de Energia**

5. **Desmarque** a opção:
   
   ```
   Permitir que o computador desligue este dispositivo para economizar energia
   ```

6. (Opcional) Vá até a aba **Avançado** → procure e ajuste:
   
   - **Energy Efficient Ethernet** → **Desativado**
   - **Green Ethernet** → **Desativado**
   - **Interrupt Moderation** → **Desativado** (em alguns adaptadores reduz latência)
     💡 *Essas opções garantem que o adaptador USB não entre em modo de economia de energia, o que pode causar quedas temporárias na conexão RDP.*

---

### 🔋 Seção 2.4 — **Ajustes de energia no sistema (fora da área remota)**

> 🔹 Essas configurações também são feitas **no computador local**  
> 🔹 Elas evitam que o sistema reduza a performance da CPU ou do adaptador USB quando está ocioso

### Passos:

1. Pressione `Win + R` → digite:
   
   ```
   control powercfg.cpl
   ```

2. Selecione o plano **Alto desempenho** (ou **Máximo desempenho**, se disponível)

3. Clique em **Alterar configurações do plano → Alterar configurações de energia avançadas**

4. Expanda e ajuste:
   
   - **Configurações do adaptador sem fio** → **Modo de economia de energia:**  
     → **Configuração: Desempenho máximo**
   
   - **Configurações USB → Configuração de suspensão seletiva USB:**  
     → **Desativado**
   
   - **Gerenciamento de energia do processador → Estado mínimo do processador:**  
     → **100%**
   
   - **PCI Express → Gerenciamento de energia do link:**  
     → **Desativado**

💡 *Essas configurações evitam oscilações na velocidade de rede e atrasos causados por economia de energia automática.*

---

## 💾 **3. Criar um arquivo `.rdp` otimizado**

Isso é útil para **carregar as configurações rapidamente** e garantir que o RDP sempre use o modo de alto desempenho.

### 🪄 Passos:

1. No mstsc, após ajustar tudo, clique em:
   
   ```
   Mostrar opções ▾ → Salvar como...
   ```

2. Salve o arquivo em:
   
   ```
   Documentos\Conexões RDP\Trabalho.rdp
   ```

3. Agora, abra o arquivo salvo com o **Bloco de Notas** e adicione (ou confirme) as seguintes linhas no final:
   
   ```ini
   screen mode id:i:2
   use multimon:i:1
   session bpp:i:16
   compression:i:1
   bitmapcachepersistenable:i:1
   disable wallpaper:i:1
   disable full window drag:i:1
   disable menu anims:i:1
   disable themes:i:1
   redirectprinters:i:0
   redirectsmartcards:i:0
   redirectcomports:i:0
   redirectdrives:i:0
   audiocapturemode:i:0
   audio redirection mode:i:1
   autoreconnection enabled:i:1
   bandwidthautodetect:i:1
   ```

4. **Para carregar essas configurações:**
   
   - Dê **duplo clique** no arquivo `.rdp` → ele abrirá o mstsc já com todas as opções aplicadas.

---

## 🌐 **4. Testar a qualidade da conexão**

Você pode medir o **atraso (delay)** real entre seu PC e o servidor remoto.

### 🧰 Fora da área remota (no seu PC):

1. Abra o **PowerShell** (não CMD) e execute:
   
   ```powershell
   Test-NetConnection -ComputerName "IP_DO_SERVIDOR" -Port 3389
   ```
   
   👉 Substitua `IP_DO_SERVIDOR` pelo IP ou domínio do servidor remoto.
   
   Observe o campo:
   
   ```
   Average RTT: X ms
   ```
   
   — quanto menor o valor, melhor (ideal: abaixo de 100 ms).

---

## 🧰 **5. Diagnóstico dentro da área remota (no servidor)**

Esses testes são executados **dentro da sessão RDP**, após estar conectado ao ambiente de trabalho remoto.

1. Abra o **PowerShell** dentro da área remota.

2. Execute o comando:
   
   ```powershell
   Get-Counter "\Terminal Services Session(*)\Round Trip Time"
   ```
   
   Isso mostra o **tempo médio de ida e volta (RTT)** da sua sessão — um indicador direto de delay. 
   
   Valores:
   
   - 🔵 0–50 ms → excelente
   
   - 🟢 50–150 ms → bom
   
   - 🟠 150–300 ms → aceitável
   
   - 🔴 >300 ms → ruim (verificar adaptador, cabos, roteador)

---

## 🧠 **6. Ajustes avançados (para uso diário)**

Se você sempre usa RDP, pode criar um **atalho direto** no desktop com o arquivo `.rdp` otimizado:

1. Clique com o botão direito → **Novo → Atalho**

2. Caminho:
   
   ```
   mstsc.exe "C:\Users\SEU_USUARIO\Documentos\Conexões RDP\Trabalho.rdp"
   ```

3. Nomeie como:
   
   ```
   Área de Trabalho Remota - Trabalho
   ```

4. Clique com o botão direito → **Propriedades → Ícone** → escolha o ícone do RDP.

---

## 🔍 **7. Dicas extras de estabilidade**

✅ **Mantenha apenas um método de rede ativo** (sem Wi-Fi e cabo ao mesmo tempo)  
✅ **Evite VPNs lentas ou cheias de usuários simultâneos**  
✅ **Evite downloads e streaming no mesmo Wi-Fi/cabo da sessão RDP**  
✅ **Use DNS rápido**, por exemplo:

```bash
1.1.1.1 (Cloudflare)
8.8.8.8 (Google)
```

Configure em **Configurações de Rede → Propriedades do adaptador → IPv4**.

---

## ⚡ **Resultado esperado após todas as otimizações**

| Área                    | Efeito esperado                      | Ganho médio |
| ----------------------- | ------------------------------------ | ----------- |
| Rede (cabo + Wi-Fi off) | Menor latência e jitter              | 🔼 Alta     |
| mstsc otimizado         | Resposta imediata ao teclado e mouse | 🔼 Alta     |
| .rdp customizado        | Desempenho consistente               | 🔼 Média    |
| RTT abaixo de 100 ms    | Cursor fluido e navegação suave      | 🔼 Alta     |
