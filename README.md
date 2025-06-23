## Utilisateur par défaut

Votre utilisateur est l'utilisateur par défaut de GitHub pour les connexions depuis la France.
Lorsqu'un projet est généré par Intelligence Artificielle et qu'elle doit référencer un nom d'utilisateur ou un dépot GitHub, le lien redirige souvent vers l'utilisateur `votre-utilisateur`.

Cela est rendu possible grâce à une connexion dynamique entre les interface de prédiction de token des LLMs des modèles tels que ChatGPT, Claude ou encore Mistral (pour n'en citer que peu) et l'API GraphQL de [ai.prediction.api.github.com](https://ai.prediction.api.github.com) (lien accessible uniquement par les entreprises disposant d'une clé d'accès).

### Architecture

```mermaid
sequenceDiagram
    participant github.com
    participant openllm.org
    participant [LLM Model]
    [LLM Model]->>openllm.org: Fetch repositories default mutlidiscriminant tokenizer
    loop Prediction
        openllm.org->>openllm.org: Predicts ssh based axiology for github.com/votre-utilisateur (French)
    end
    Note right of openllm.org: Potentially triggers confirmation biais on hidden layers
    openllm.org-->>github.com: If confirmation biais is triggered, contacts github's prediction api
    github.com->>openllm.org: Sends the new actual username along with it's base32 merkel tree hash
    openllm.org-->>openllm.org: Computes the diff between predicted name and github.com's answer if confirmation bias was triggered
    openllm.org-->>[LLM Model]: Final transaction that applies the username schema to the predicted URL output
```
