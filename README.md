**Netflix Shows & Movies**

**Aim**

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

**Procedure / Algorithm**

  1)Load dataset (netflix_titles.csv).
  
  2)Count movies vs TV shows.
  
  3)Group by country → top contributors.
  
  4)Create pivot table (release year vs type).
  
  5)Visualize with bar & line charts.

**Program**


    import pandas as pd
    url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
    df = pd.read_csv(url)
    
    
    print("Dataset Shape:", df.shape)
    print("\nColumns:\n", df.columns)
    print("\nType Counts:\n", df['type'].value_counts())
    
    df['date_added'] = df['date_added'].astype(str).str.strip()
    df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')
    
    df['year_added'] = df['date_added'].dt.year
    df['month_added'] = df['date_added'].dt.month_name()
    
    count_by_type = df.groupby('type')['title'].count()
    print("\nCount by Type:\n", count_by_type)
    
    
    pivot_country_type = df.pivot_table(
        index='country',
        columns='type',
        values='title',
        aggfunc='count',
        fill_value=0
    )
    
    
    pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
    
    
    max_country = pivot_country_type['Total'].idxmax()
    max_count = pivot_country_type['Total'].max()
    
    print("\nCountry vs Type Pivot Table (Sample):\n", pivot_country_type.head())
    print(f"\nCountry with most titles: {max_country} ({max_count})")
    
    
    top_directors = df['director'].value_counts().head(5)
    print("\nTop 5 Directors:\n", top_directors)
    
    
    trend = df.groupby(['year_added', 'type']).size().unstack(fill_value=0)
    print("\nYearly Trend (Sample):\n", trend.head())
    
    
    df_genre = (
        df[['show_id', 'listed_in']]
        .dropna()
        .assign(listed_in=df['listed_in'].str.split(', '))
        .explode('listed_in')
    )
    
    
    df_expanded = df.merge(df_genre, on='show_id', how='left')
    
    print("\nColumns after merge:\n", df_expanded.columns)
    
    print("\nExpanded Genre Sample:\n",
          df_expanded[['title', 'listed_in_y']].head())
    
    
    top_genres = df_expanded['listed_in_y'].value_counts().head(5)
    print("\nTop 5 Genres:\n", top_genres)
    

**Ouptut**

<img width="1022" height="837" alt="Screenshot 2026-02-08 205710" src="https://github.com/user-attachments/assets/a5feb952-cf24-4f18-b78a-df6bfec688b1" />


<img width="934" height="680" alt="Screenshot 2026-02-08 205721" src="https://github.com/user-attachments/assets/603b7871-7a84-4619-b1d2-0b7e87c99c94" />

**Result**
Helps Netflix in content planning & investments.
