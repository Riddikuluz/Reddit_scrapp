# Análisis de Sentimientos en Reddit

Este proyecto utiliza la biblioteca `praw` para interactuar con la API de Reddit y realizar un análisis de sentimientos en las publicaciones y comentarios del subreddit 'chile'. El análisis de sentimientos se realiza utilizando la biblioteca `sentiment_analysis_spanish`.

## Instalación

Para instalar las bibliotecas necesarias, ejecuta el siguiente comando:

```shell
%pip install praw sentiment-analysis-spanish python-dotenv
```

## Uso

Primero, carga tus variables de entorno desde un archivo `.env`. Estas deben incluir el `client_id`, `client_secret` y `user_agent` de tu aplicación de Reddit.

```python
from dotenv import load_dotenv
import os

load_dotenv()

client_id = os.getenv('REDDIT_CLIENT_ID')
client_secret = os.getenv('REDDIT_CLIENT_SECRET')
user_agent = os.getenv('REDDIT_USER_AGENT')
```

Luego, crea una instancia de `praw.Reddit` con las credenciales de tu aplicación:

```python
import praw

reddit = praw.Reddit(
   client_id=client_id,
   client_secret=client_secret,
   user_agent=user_agent
)
```

El script extrae las 300 publicaciones más populares y sus comentarios del subreddit 'chile', realiza un análisis de sentimientos sobre ellos y guarda los resultados en un archivo CSV. El análisis de sentimientos etiqueta cada publicación o comentario como 'Positivo', 'Neutral' o 'Negativo'.

```python
# Para las publicaciones:
sentiment = sentiment_analysis.SentimentAnalysisSpanish()
hot_posts = reddit.subreddit('chile').hot(limit=300)
data = []
for post in hot_posts:
    # Realiza el análisis de sentimientos y guarda los resultados...
df = pd.DataFrame(data)
df.to_csv('reddit_post.csv', index=False)
```
```python
# Para los comentarios:
sentiment = sentiment_analysis.SentimentAnalysisSpanish()
hot_posts = reddit.subreddit('chile').hot(limit=300)
data = []
for post in hot_posts:
    post.comments.replace_more(limit=None)
    for comment in post.comments.list():
        # Realiza el análisis de sentimientos y guarda los resultados...
df = pd.DataFrame(data)
df.to_csv('reddit_comments.csv', index=False)
```

## Nota

Recuerda reemplazar `'your-client-id'`, `'your-client-secret'`, y `'your-user-agent'` con las credenciales reales de tu aplicación de Reddit. Además, asegúrate de que tu archivo `.env` esté en el mismo directorio que tu script de Python.

Si sigues teniendo problemas, ¡házmelo saber! Estoy aquí para ayudar. 😊
