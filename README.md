# Análisis de Sentimientos en Reddit

Este proyecto utiliza la biblioteca `praw` para interactuar con la API de Reddit y realizar un análisis de sentimientos en las publicaciones y comentarios del subreddit 'chile'. El análisis de sentimientos se realiza utilizando la biblioteca `sentiment_analysis_spanish`.

## Autores

Este repositorio fue desarrollado por el grupo compuesto por:

```bash
Jean Agurto Navarrete
Romina Araya Araya
Francisco Robledo Venegas
```
## Instalación

Para instalar las bibliotecas necesarias, ejecuta el siguiente comando:

```shell
%pip install praw sentiment-analysis-spanish python-dotenv openpyxl
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

El script extrae las 450 publicaciones más populares y sus comentarios del subreddit 'chile', realiza un análisis de sentimientos sobre ellos y guarda los resultados en un solo archivo XLSX. El análisis de sentimientos etiqueta cada publicación o comentario como 'Positivo', 'Neutral' o 'Negativo'.

```python
# Crea una instancia del analizador de sentimientos
sentiment = sentiment_analysis.SentimentAnalysisSpanish()

# Descarga los posts más populares del subreddit 'chile'
hot_posts = reddit.subreddit('chile').hot(limit=450)

data = []
count = 0
n_comments=450

for post in hot_posts:
    if count >= n_comments:
        break

    # Analiza el sentimiento del título del post y guarda los resultados
    if post.title:
        data.append(create_data_dict(post.title, post.created, post.author.name if post.author else '', post.permalink))
        count += 1

    # Extrae los comentarios del post
    post.comments.replace_more(limit=None)
    for comment in post.comments.list():
        if count >= n_comments:
            break

        # Analiza el sentimiento del comentario y guarda los resultados
        if comment.body:
            data.append(create_data_dict(comment.body, comment.created, comment.author.name if comment.author else '', comment.permalink))
            count += 1

# Crea un DataFrame con los datos
df = pd.DataFrame(data)

# Guarda el dataframe en un archivo CSV
# df.to_csv('reddit_data.csv', index=False)

# Guarda el dataframe en un archivo Excel
df.to_excel('reddit_data.xlsx', index=False)
```

## Nota

Recuerda reemplazar `'your-client-id'`, `'your-client-secret'`, y `'your-user-agent'` con las credenciales reales de tu aplicación de Reddit. Además, asegúrate de que tu archivo `.env` esté en el mismo directorio que tu script de Python.