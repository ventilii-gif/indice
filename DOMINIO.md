# ventilii.ai — configurazione

## Struttura

- `https://ventilii.ai` → questa repo (`indice`), la home
- `https://<progetto>.ventilii.ai` → un sottodominio per ogni progetto
- `https://ventilii.ai/<progetto>/` → cartella che reindirizza al sottodominio,
  così i vecchi link continuano a funzionare

## DNS su Namecheap

| Type  | Host  | Value                     |
|-------|-------|---------------------------|
| A     | `@`   | `185.199.108.153`         |
| A     | `@`   | `185.199.109.153`         |
| A     | `@`   | `185.199.110.153`         |
| A     | `@`   | `185.199.111.153`         |
| AAAA  | `@`   | `2606:50c0:8000::153`     |
| AAAA  | `@`   | `2606:50c0:8001::153`     |
| AAAA  | `@`   | `2606:50c0:8002::153`     |
| AAAA  | `@`   | `2606:50c0:8003::153`     |
| CNAME | `www` | `ventilii-gif.github.io.` |
| CNAME | `*`   | `ventilii-gif.github.io.` |

Il record **wildcard `*`** copre qualunque sottodominio, presente e futuro:
non serve toccare Namecheap per aggiungerne di nuovi.

Restano in zona anche i record MX (`eforward1-5.registrar-servers.com`) e il
TXT SPF dell'inoltro email di Namecheap: non c'entrano col sito, non vanno
rimossi.

## Aggiungere un progetto nuovo

1. nella repo del progetto, file `CNAME` in root con dentro `nome.ventilii.ai`
   (le repo dei progetti pubblicano da branch, quindi il file basta: imposta
   il dominio da solo, senza passare dalle impostazioni)
2. in questa repo, una scheda nell'indice che punta a `https://nome.ventilii.ai/`
3. in questa repo, la cartella `nome/index.html` col reindirizzamento

Nessun passaggio DNS.

## Nota sulla pubblicazione di questa repo

`indice` pubblica tramite il workflow `.github/workflows/pages.yml`, non da
branch. Con quel metodo GitHub **ignora il file `CNAME`**: il dominio
personalizzato di questa repo si imposta in Settings → Pages → Custom domain.

## Protezione da takeover (consigliata)

Il wildcard permette in teoria ad altri account GitHub di rivendicare un
sottodominio non usato. Si blocca verificando il dominio:
GitHub → Settings → Pages → **Add a domain** → `ventilii.ai` → copiare il
record TXT proposto (`_github-pages-challenge-ventilii-gif`) nella zona DNS
→ Verify.
