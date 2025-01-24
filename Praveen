# def scrape_imdb_bollywood(base_url, pages=1):
    movie_data = []
    
    for page in range(pages):
        url = f"{base_url}&start={page * 50 + 1}"
        response = requests.get(url)
        soup = BeautifulSoup(response.content, 'html.parser')

        # Find all movie containers
        movies = soup.find_all('div', class_='lister-item mode-advanced')
        
        for movie in movies:
            title = movie.h3.a.text.strip()  # Movie title
            year = movie.h3.find('span', class_='lister-item-year').text.strip("()")
            genre = movie.find('span', class_='genre').text.strip() if movie.find('span', class_='genre') else None
            imdb_rating = movie.find('div', class_='inline-block ratings-imdb-rating')
            imdb_rating = float(imdb_rating['data-value']) if imdb_rating else None
            votes = movie.find_all('span', attrs={'name': 'nv'})
            votes = int(votes[0]['data-value'].replace(",", "")) if votes else None
            try:
                runtime = movie.find('span', class_='runtime').text.strip(" min")
                runtime = int(runtime)
            except AttributeError:
                runtime = None
            
            movie_data.append({
                'Title': title,
                'Year': year,
                'Genre': genre,
                'IMDb_Rating': imdb_rating,
                'Votes': votes,
                'Runtime (mins)': runtime
            })

    return pd.DataFrame(movie_data)
