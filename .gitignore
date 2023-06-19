import requests
from bs4 import BeautifulSoup
import sqlite3

# Step 1: Choose Recipe Websites to Scrape
recipe_websites = ['https://www.example.com/recipes', 'https://www.anotherexample.com/recipes']

# Step 2: Scrape Recipe Data
def scrape_recipe_data(url):
    response = requests.get(url)
    response.raise_for_status()
    soup = BeautifulSoup(response.text, 'html.parser')

    # Extract recipe information from the website
    recipe_name = soup.find('h1', class_='recipe-name').text.strip()
    ingredients = soup.find('ul', class_='ingredients-list').find_all('li')
    ingredient_list = [ingredient.text.strip() for ingredient in ingredients]
    instructions = soup.find('div', class_='instructions').text.strip()
    cooking_time = soup.find('span', class_='cooking-time').text.strip()

    # Return the scraped recipe data as a dictionary
    return {
        'recipe_name': recipe_name,
        'ingredients': ingredient_list,
        'instructions': instructions,
        'cooking_time': cooking_time
    }

# Step 3: Set Up a Database
connection = sqlite3.connect('recipes.db')
cursor = connection.cursor()

# Create a table to store recipe data
cursor.execute('''
    CREATE TABLE IF NOT EXISTS recipes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        recipe_name TEXT,
        ingredients TEXT,
        instructions TEXT,
        cooking_time TEXT
    )
''')

# Step 4: Scrape Recipe Websites and Store Data in the Database
for website in recipe_websites:
    recipes = scrape_recipe_data(website)
    for recipe in recipes:
        cursor.execute('''
            INSERT INTO recipes (recipe_name, ingredients, instructions, cooking_time)
            VALUES (?, ?, ?, ?)
        ''', (recipe['recipe_name'], ', '.join(recipe['ingredients']), recipe['instructions'], recipe['cooking_time']))

# Commit the changes and close the database connection
connection.commit()
connection.close()

# Step 5: Design User Preference Interface
def get_recipe_recommendations(dietary_restrictions, available_ingredients):
    # Step 6: Implement Recipe Recommendation Logic
    connection = sqlite3.connect('recipes.db')
    cursor = connection.cursor()

    # Build the SQL query based on user preferences
    query = 'SELECT * FROM recipes WHERE '
    conditions = []
    for restriction in dietary_restrictions:
        conditions.append(f"ingredients NOT LIKE '%{restriction}%'")
    if conditions:
        query += ' AND '.join(conditions)

    # Retrieve recipes matching the user's preferences
    cursor.execute(query)
    recommended_recipes = cursor.fetchall()

    # Step 7: Display Recipe Recommendations
    for recipe in recommended_recipes:
        print('Recipe Name:', recipe[1])
        print('Ingredients:', recipe[2])
        print('Instructions:', recipe[3])
        print('Cooking Time:', recipe[4])
        print()

    # Close the database connection
    connection.close()

# Example Usage
dietary_restrictions = ['dairy', 'gluten']
available_ingredients = ['chicken', 'broccoli']

get_recipe_recommendations(dietary_restrictions, available_ingredients)
