# ventilii.ai — configurazione

## Struttura

- `https://ventilii.ai` → questa repo (`indice`), la home
- `https://<progetto>.ventilii.ai` → un sottodominio per ogni progetto
- `https://ventilii.ai/<progetto>/` → cartella che reindirizza al sottodominio,
  così i vecchi link continuano a funzionare

## DNS su Namecheap

| Type  | Host                                   | Value                          |
|-------|----------------------------------------|--------------------------------|
| A     | `@`                                    | `185.199.108.153`              |
| A     | `@`                                    | `185.199.109.153`              |
| A     | `@`                                    | `185.199.110.153`              |
| A     | `@`                                    | `185.199.111.153`              |
| AAAA  | `@`                                    | `2606:50c0:8000::153`          |
| AAAA  | `@`                                    | `2606:50c0:8001::153`          |
| AAAA  | `@`                                    | `2606:50c0:8002::153`          |
| AAAA  | `@`                                    | `2606:50c0:8003::153`          |
| CNAME | `www`                                  | `ventilii-gif.github.io.`      |
| CNAME | `*`                                    | `ventilii-gif.github.io.`      |
| TXT   | `_github-pages-challenge-ventilii-gif` | codice di verifica del dominio |

Il record **wildcard `*`** copre qualunque sottodominio, presente e futuro:
per aggiungerne uno non serve toccare il DNS. Restano in zona anche i record
MX (`eforward1-5.registrar-servers.com`) e il TXT SPF dell'inoltro email di
Namecheap: non c'entrano col sito e non vanno rimossi.

Il dominio è verificato su GitHub (Settings → Pages → Add a domain), il che
impedisce ad altri account di rivendicare un sottodominio non usato — cosa
che il wildcard da solo permetterebbe.

## Aggiungere un progetto: ATTENZIONE al metodo di pubblicazione

Il passaggio 1 dipende da **come la repo pubblica su Pages**, e le due
strade non sono equivalenti:

- **Pubblicazione da branch** (`Deploy from a branch`) → basta un file
  `CNAME` in root con dentro `nome.ventilii.ai`. GitHub lo legge e imposta
  il dominio da solo.
- **Pubblicazione via Actions** (`GitHub Actions`) → **GitHub ignora il file
  `CNAME`**. Il dominio va scritto a mano in Settings → Pages → Custom
  domain. È un'operazione riservata al proprietario dell'account: non è
  raggiungibile né dalle API di un agente né dal token del workflow (che
  risponde `403 Resource not accessible by integration` anche con
  `pages: write`).

Il metodo si vede in Settings → Pages, riquadro "Build and deployment",
voce Source. **Non dedurlo dalla presenza di un file in
`.github/workflows/`**: alcune repo hanno un workflow avanzato ma
pubblicano comunque da branch.

Repo che al 29/08/2026 pubblicano via Actions, e che quindi richiedono il
passaggio manuale: `indice`, `Algebra-lineare`, `Geometria-euclidea`,
`Piano-cartesiano-1`, `dinamica`, `naturali`, `termodinamica`.

Poi, in ogni caso:

2. in questa repo, una scheda nell'indice che punta a `https://nome.ventilii.ai/`
3. in questa repo, la cartella `nome/index.html` col reindirizzamento

Nessun passaggio DNS.

## HTTPS

Il certificato lo emette Let's Encrypt su richiesta di GitHub, qualche minuto
dopo che il dominio è stato impostato e il controllo DNS è passato. Finché non
arriva, la casella **Enforce HTTPS** resta grigia: è normale, va solo aspettato.

Due avvertenze:

- Non rimuovere e rimettere il dominio per "sbloccare" il certificato: ogni
  salvataggio fa ripartire una richiesta, e Let's Encrypt ha un tetto di
  50 certificati a settimana **per dominio registrato**, sottodomini inclusi.
  Con ~40 progetti quel tetto si avvicina in fretta.
- Un sottodominio inesistente (es. `qualsiasi.ventilii.ai`) risponde con un
  404 di GitHub invece che con un errore del browser: è il prezzo del
  wildcard, non un guasto.

## Nota su questa repo

`indice` pubblica tramite `.github/workflows/pages.yml` dal branch
`claude/busy-cannon-7t5irh`, che è anche il branch di default. Le modifiche
diventano visibili solo quando arrivano lì.
