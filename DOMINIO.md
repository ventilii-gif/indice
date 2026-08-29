# ventilii.ai — configurazione

## Come funziona

Il dominio è agganciato **una volta sola** al *user site* di GitHub Pages
(repo `ventilii-gif.github.io`). Da lì GitHub serve automaticamente **tutti**
gli altri repo dell'account sotto lo stesso dominio, senza altra configurazione:

| Repo                     | URL pubblico                       |
|--------------------------|------------------------------------|
| `ventilii-gif.github.io` | `https://ventilii.ai/`             |
| `acustica`               | `https://ventilii.ai/acustica/`    |
| `derivate`               | `https://ventilii.ai/derivate/`    |
| *ogni altro repo*        | `https://ventilii.ai/<nome-repo>/` |

Nessun record DNS per progetto, nessun file da aggiungere nei 48 repo: è il
comportamento nativo di GitHub Pages (un *project site* eredita il dominio
del *user site*).

## DNS su Namecheap — FATTO

Zona `ventilii.ai` (nameserver Namecheap di default):

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

Il record **wildcard `*`** copre in anticipo qualunque sottodominio futuro
(`fisica.ventilii.ai`, `quiz.ventilii.ai`, ...): non serve più rientrare in
Namecheap per aggiungerne.

## Sottodomini (quando serviranno)

Con il wildcard già attivo, spostare un progetto su un sottodominio è
un'operazione di solo GitHub, due passi per repo:

1. file `CNAME` in root del repo, con dentro `nome.ventilii.ai`
2. Settings → Pages → Custom domain → `nome.ventilii.ai`

## Protezione da takeover (consigliata)

Il wildcard permette in teoria ad altri account GitHub di rivendicare un
sottodominio non usato. Si blocca verificando il dominio:
GitHub → Settings → Pages → **Add a domain** → `ventilii.ai` → copiare il
record TXT proposto (`_github-pages-challenge-ventilii-gif`) nella zona DNS
→ Verify.
