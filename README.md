# Financial News Sentiment Analysis

A comprehensive sentiment analysis system that processes financial news articles from a SQLite database using multiple state-of-the-art models and provides ensemble predictions for stock sentiment analysis.

## 🚀 Features

- **Multi-Model Ensemble**: Combines predictions from 7 different sentiment analysis models
- **Database Integration**: Reads news data from SQLite database
- **API Validation**: Compares results with external sentiment API for accuracy assessment
- **Comprehensive Preprocessing**: Cleans and prepares text data for analysis
- **Weighted Scoring**: Uses sophisticated weighting system for optimal accuracy

## 📊 Models Used

The system employs the following models with different weights in the ensemble:

| Model | Weight | Description |
|-------|---------|-------------|
| FinBERT | 4 | Financial domain-specific BERT model |
| FinBERT Tone | 3 | Specialized for financial tone analysis |
| Sentiment RoBERTa | 3 | Large English sentiment analysis model |
| DistilRoBERTa | 2 | Financial news sentiment analysis |
| FinTwitBERT | 1 | Twitter financial sentiment model |
| DistilBERT | 1 | General sentiment analysis |
| VADER | 1 | Lexicon and rule-based sentiment analyzer |

## 🛠️ Installation

### Prerequisites

```bash
pip install sqlite3
pip install nltk
pip install transformers
pip install pandas
pip install scikit-learn
pip install certifi
```

### NLTK Data

The system will automatically download the required VADER lexicon:

```python
import nltk
nltk.download('vader_lexicon')
```

## 📁 Project Structure

```
├── Sentiment_for_newsdb.ipynb    # Main Jupyter notebook
├── News.db                       # SQLite database with news data
├── README.md                     # This file
└── requirements.txt              # Python dependencies
```

## 🔧 Usage

### Basic Usage

```python
from sentiment_analyzer import *

# Initialize the system
db_path = "News.db"
query = "SELECT * FROM stock_news"

# Fetch and analyze news data
news_data = fetch_news_from_db(db_path, query)
process_news_articles(news_data)
```

### Database Schema

The system expects a SQLite database with a table named `stock_news` containing:

- `title` (TEXT): News headline
- `text` (TEXT): News content/snippet
- Additional metadata columns (optional)

### API Integration

Compare your results with external sentiment API:

```python
# Fetch API sentiments for validation
api_url = "https://financialmodelingprep.com/api/v4/stock-news-sentiments-rss-feed?page=0&apikey=YOUR_API_KEY"
api_sentiments = get_jsonparsed_data(api_url)
```

## 🎯 Core Functions

### `analyze_sentiment(text: str)`
Analyzes sentiment using all available models and returns normalized scores.

### `ensemble_sentiment(sentiments: List[Tuple[str, Dict]])`
Calculates weighted ensemble score from individual model predictions.

### `process_news_articles(news_data: List[Dict])`
Processes a list of news articles and displays sentiment analysis results.

### `fetch_news_from_db(db_path: str, query: str)`
Retrieves news data from SQLite database.

## 📈 Performance Metrics

The system has been tested with various weight configurations:

- **Best Configuration**: FinBERT(4), FinBERT Tone(3), Sentiment RoBERTa(3), Others(2,1,1,1)
- **Accuracy Range**: 23% - 44% depending on weight configuration
- **Optimal Weights**: Prioritize financial domain-specific models

## 🔍 Example Output

```
--------------------------------------------------------------------------------
Headline: AAPL
Snippet: Apple gains after Morgan Stanley calls stock 'top pick' for AI efforts
Combined Ensemble Sentiment Score: 0.80 (POSITIVE)
Combined Sentiments:
  - VADER: POSITIVE (score: 0.49)
  - FinBERT: POSITIVE (score: 0.78)
  - DistilBERT: NEGATIVE (score: -0.52)
  - FinBERT Tone: POSITIVE (score: 1.00)
  - DistilRoberta: POSITIVE (score: 1.00)
  - FinTwitBERT: BULLISH (score: 0.94)
  - Sentiment Roberta: POSITIVE (score: 1.00)
--------------------------------------------------------------------------------
```

## ⚙️ Configuration

### Adjusting Model Weights

Modify the weights in the `ensemble_sentiment` function:

```python
weights = {
    'FinBERT': 4,           # Primary financial model
    'FinBERT Tone': 3,      # Financial tone analysis
    'Sentiment Roberta': 3, # General sentiment (high quality)
    'DistilRoberta': 2,     # Financial news specific
    'FinTwitBERT': 1,       # Social media financial sentiment
    'DistilBERT': 1,        # General purpose
    'VADER': 1              # Rule-based baseline
}
```

### Text Preprocessing

The system automatically:
- Removes HTML tags
- Strips URLs
- Normalizes whitespace
- Handles special characters

## 🚫 Limitations

- **Model Loading Time**: Initial loading of transformer models takes time
- **Memory Usage**: Multiple large models require significant RAM
- **API Dependencies**: External validation requires API access
- **Language Support**: Optimized for English financial text

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🔗 References

- [FinBERT: Financial Sentiment Analysis](https://huggingface.co/ProsusAI/finbert)
- [VADER Sentiment Analysis](https://github.com/cjhutto/vaderSentiment)
- [Transformers Library](https://huggingface.co/transformers/)

## 🆘 Support

For questions, issues, or contributions:

1. Check existing issues in the repository
2. Create a new issue with detailed description
3. Include sample data and error messages
4. Specify your environment details

## 📊 Version History

- **v1.0.0**: Initial release with multi-model ensemble
- **v1.1.0**: Added API validation and accuracy metrics
- **v1.2.0**: Optimized weights and improved preprocessing

---

**Note**: This system is designed for financial news sentiment analysis. Results should be used as part of a broader analysis framework and not as sole investment advice.
