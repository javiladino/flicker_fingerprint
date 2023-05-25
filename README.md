# Create a chromatic fingerprint from Flickr using Python

Create a chromatic fingerprint from Flickr using Python
Do you love art and colors? Do you want to explore the visual world through programming? In this project, I will show you how to create the chromatic fingerprint of some cities in France using Python and the Flickr API. Get ready for a journey full of vibrant colors and creativity!

Nantes
Introduction: In this tutorial, you will learn how to use Python to perform a Flickr search and create a color wheel using the images found. The color wheel will display a visual representation of the average colors of the photos related to the Flickr search.

Conditions préalables :

Python 3 installé sur votre ordinateur.
Les bibliothèques Requests et Pillow (l’implémentation de PIL) sont installées. Vous pouvez les installer en lançant pip install requests pillow dans votre terminal.

Étape 1 : Obtenir les clés de l’API Flickr
Pour utiliser l’API Flickr, nous devons obtenir les clés de l’API. Vous pouvez les obtenir en vous enregistrant en tant que développeur sur le site web de Flickr (https://www.flickr.com/services/api/).
Une fois que vous avez obtenu les clés API (API Key et API Secret), remplacez “TU_API_KEY” et “TU_API_SECRET” dans le code Python par vos propres clés.

Étape 2 : Écrire le code pour rechercher et télécharger les photos
import requests
from io import BytesIO
from PIL import Image, ImageDraw
import webbrowser

# Flickr API Keys
api_key = "YOUR_API_KEY"
api_secret = "YOUR_API_SECRET"

# Perform a search on Flickr
def search_photos(query):
    url = "https://api.flickr.com/services/rest/"
    params = {
        "method": "flickr.photos.search",
        "api_key": api_key,
        "text": query,
        "sort": "relevance",
        "per_page": 24,
        "format": "json",
        "nojsoncallback": 1
    }
    response = requests.get(url, params=params)
    data = response.json()
    return data["photos"]["photo"]

# Download a photo from Flickr
def download_photo(photo):
    url = "https://farm{farm}.staticflickr.com/{server}/{id}_{secret}.jpg".format(**photo)
    response = requests.get(url)
    return Image.open(BytesIO(response.content))
In the above code, we use the API keys to perform a search on Flickr and get the data of the photos related to the search. Then, we download the photos using the URL provided by Flickr and convert them into image objects using PIL.


Step 3: Create the color wheel

# Create a color wheel based on the downloaded photos
def create_color_wheel(photos):
    size = (800, 800)
    wheel = Image.new("RGB", size)
    draw = ImageDraw.Draw(wheel)

    angle = 0
    angle_increment = 360 / len(photos)

    for photo in photos:
        image = download_photo(photo)
        image = image.resize((100, 100))

        averaged_image = image.resize((1, 1), Image.ANTIALIAS)
        average_color = averaged_image.getpixel((0, 0))
        
        draw.pieslice([(0, 0), size], angle, angle + angle_increment, fill=average_color)
        angle += angle_increment

    wheel.show()

# Perform a search on Flickr and create the color wheel
search_term = input("Enter your search term on Flickr: ")
photos = search_photos(search_term)
create_color_wheel(photos)
Toulouse
This code performs a search on Flickr based on a term entered by the user, downloads the first 100 related photos and creates a color wheel using the average colors of each photo. You can modify the number of photos (“per_page“) but note that the Flickr API has a request limit. The resulting color wheel is displayed in a popup window.
