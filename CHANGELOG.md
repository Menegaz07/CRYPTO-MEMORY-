# 🎮 Crypto Memory — Histórico de Alterações

**🔗 App no ar:** https://menegaz07.github.io/CRYPTO-MEMORY-/

Jogo educativo da memória sobre criptomoedas — 100% estático (HTML único), com preços em tempo real e áudio.

---

## Sessão de 26/06/2026 — Áudio e Preços

### 🔊 Áudio
- **Volume da música da vitória no celular (iOS):** o iPhone/iPad ignora `.volume` de elementos `<audio>`. A música agora é roteada pelo Web Audio API (`createMediaElementSource` → `GainNode` → destino), tornando o volume controlável no celular. Fallback ao `.volume` no desktop.
- **Controle de volume calibrado:** o slider passou a usar uma **curva perceptual** (`gain = posição^2.5`), pois o ouvido é logarítmico — antes (linear) quase toda a faixa audível ficava espremida perto do mute. Agora há controle fino no volume baixo.
- **Volume da música da vitória:** reduzida para **50%** do nível dos efeitos (≈ −6 dB).
- **Duração da música da vitória:** reduzida de **30s → 15s**.

### 💰 Preços (CoinGecko)
- **Resiliência a falha/rate-limit (429):**
  - Cache da última cotação em `localStorage` (aparece na hora ao abrir).
  - Retry automático com backoff (2s → 30s) no carregamento.
  - Em caso de falha, mantém a última cotação em vez de mostrar "indisponível".
- Atualização automática a cada **30s**.

### 📦 Commits da sessão
| Commit | Descrição |
|--------|-----------|
| `84724bd` | Volume da música da vitória funciona no iOS (Web Audio GainNode) |
| `994f284` | Carregamento de preços resiliente (cache + retry com backoff) |
| `59003cb` | Controle de volume com curva perceptual (`gain = pos^2.5`) |
| `8c76e54` | Música da vitória reduzida em 10% (fator 0.9) |
| `37c5d73` | Música da vitória reduzida mais 10% (fator 0.8) |
| `e3739f4` | Música da vitória a 50% (≈ −6 dB) |
| `59e24ce` | Duração da música da vitória reduzida de 30s → 15s |

---

## 🔧 Constantes para ajuste rápido (no `index.html`)
| Constante / valor | Função |
|-------------------|--------|
| `END_SOUND_FACTOR = 0.5` | Volume da música da vitória (relativo aos efeitos) |
| expoente `2.5` em `gainFor()` | Sensibilidade da curva do slider de volume |
| `15000` no `sennaTimeoutId` | Duração (ms) da música da vitória |
| `PRICE_REFRESH_MS = 30000` | Frequência (ms) de atualização dos preços |

---

## 🛣️ Possíveis melhorias futuras
- Chave **Demo gratuita** do CoinGecko (header `x-cg-demo-api-key`) para eliminar o rate-limit (429) de vez — requer ajustar a CSP.
- Tag/release no Git marcando versões.
