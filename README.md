# market
name: Auto Scrape

# on:
#   push:
#     branches:
#       - master

on:
  push:
    branches:
      - main
  schedule:
    - cron: '15 10 * * *' # Every Day at 4PM NST (10:15am UTC)

jobs:

  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout 
        uses: actions/checkout@v2

      - name: Setup Python Environment
        uses: actions/setup-python@v2
        with:
          python-version: 3.7
      
      - name: Install Dependencies
        run: |
          pip install -r requirements.txt
      
      - name: Execute Scraping Script
        run: |
          python scrape_floorsheet.py
      - name: Commit and Push Files
        run: |
           git config --local user.email "suyogdahal46@gmail.com"
           git config --local user.name "suyogdahal"        
           git add .
           git commit -m "Data Scraped Sucessfully" -a
           git push origin main
      
      # - name: Push changes
      #   uses: ad-m/github-push-action@main
      #   with:
      #     github_token: ${{ secrets.GITHUB_TOKEN }}
