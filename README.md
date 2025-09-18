# Week Assignment: Build an AI Data Collection Agent

**Course:** Data Analysis  
**Week:** [Week Number]  
**Due Date:** [Insert Date]  
**Points:** 100 points  

---

## Assignment Overview

In this assignment, you will create an AI-powered data collection agent that automatically gathers, processes, and documents data according to a Data Management Plan. You'll learn to work with APIs, implement respectful data collection practices, and build an intelligent system that can adapt its collection strategy based on data quality and availability.

**Learning Objectives:**
- Understand and implement API interactions from scratch
- Apply Data Management Plan principles in practice
- Build an intelligent agent that makes data collection decisions
- Practice ethical and respectful data collection
- Create comprehensive documentation and quality checks

---

## What You'll Build

Your AI agent will:
1. **Plan**: Read your Data Management Plan and understand collection requirements
2. **Collect**: Gather data from multiple free APIs using intelligent strategies
3. **Process**: Clean, validate, and organize collected data
4. **Document**: Automatically generate metadata and quality reports
5. **Adapt**: Adjust collection strategy based on success rates and data quality

---

## Part 1: Choose Your Data Collection Scenario (20 points)

Select ONE of the following scenarios and create a mini Data Management Plan:

### Scenario A: Weather Pattern Analysis
**Objective:** Collect weather data to analyze climate patterns across different cities
- **Data Sources:** OpenWeatherMap API (free tier), WeatherAPI (free tier)
- **Data Types:** Temperature, humidity, precipitation, wind speed
- **Geographic Scope:** 5-10 cities of your choice
- **Time Range:** Current conditions + 5-day forecast

### Scenario B: News Sentiment Tracker
**Objective:** Monitor news sentiment on a topic of interest
- **Data Sources:** NewsAPI (free tier), Reddit API (free)
- **Data Types:** Headlines, article summaries, publication dates, sources
- **Topic:** Choose any current event or topic (climate change, technology, sports, etc.)
- **Scope:** English language articles from past week

### Scenario C: GitHub Repository Analytics
**Objective:** Analyze open source project trends and activity
- **Data Sources:** GitHub API (free, high rate limits)
- **Data Types:** Repository info, commit activity, contributor stats, issue counts
- **Focus:** Choose a programming language or topic area
- **Scope:** Top 20-50 repositories matching your criteria

### Scenario D: Economic Indicators Dashboard
**Objective:** Track economic indicators and market trends
- **Data Sources:** Alpha Vantage API (free tier), Federal Reserve Economic Data (FRED)
- **Data Types:** Stock prices, exchange rates, economic indicators
- **Scope:** 3-5 stocks or currencies + 2-3 economic indicators
- **Time Range:** Daily data for past month

**Deliverable 1:** Mini Data Management Plan (1-2 pages) including:
- Research question and objectives
- Data sources and types
- Collection strategy and timeline
- Quality assurance plan
- Documentation approach

---

## Part 2: API Fundamentals - Your First API Call (15 points)

Before building your agent, you'll learn API basics through guided exercises.

### Exercise 2.1: Understanding APIs

**What is an API?**
An API (Application Programming Interface) is like a waiter in a restaurant:
- You (the client) make a request for what you want
- The waiter (API) takes your request to the kitchen (server)
- The kitchen prepares your order (processes data)
- The waiter brings back your food (returns data)

### Exercise 2.2: Your First API Call

Let's start with a simple example using a free API that requires no authentication:

```python
import requests
import json

# Make your first API call to get a random cat fact
def get_cat_fact():
    url = "https://catfact.ninja/fact"
    
    try:
        # Send GET request to the API
        response = requests.get(url)
        
        # Check if request was successful
        if response.status_code == 200:
            # Parse JSON response
            data = response.json()
            return data['fact']
        else:
            print(f"Error: {response.status_code}")
            return None
    
    except Exception as e:
        print(f"An error occurred: {e}")
        return None

# Test your function
cat_fact = get_cat_fact()
print(f"Cat fact: {cat_fact}")
```

**Your Task:**
1. Run this code and understand each part
2. Modify it to get 5 different cat facts
3. Add proper error handling and logging
4. Save the facts to a JSON file

### Exercise 2.3: API with Parameters

Now let's try an API that accepts parameters:

```python
import requests

def get_public_holidays(country_code="US", year=2024):
    """
    Get public holidays for a specific country and year
    Uses Nager.Date API (free, no key required)
    """
    url = f"https://date.nager.at/api/v3/PublicHolidays/{year}/{country_code}"
    
    try:
        response = requests.get(url)
        response.raise_for_status()  # Raises an exception for bad status codes
        
        holidays = response.json()
        return holidays
    
    except requests.exceptions.RequestException as e:
        print(f"Request failed: {e}")
        return None

# Test with different countries
countries = ['US', 'CA', 'GB', 'FR', 'JP']
for country in countries:
    holidays = get_public_holidays(country)
    if holidays:
        print(f"{country} has {len(holidays)} public holidays in 2024")
```

**Your Task:**
1. Test this function with 3 different countries
2. Extract and print just the holiday names and dates
3. Create a summary comparing holiday counts by country

**Deliverable 2:** 
- Working code files for both exercises
- Brief reflection (1 paragraph) on what you learned about APIs

---

## Part 3: Setting Up Free API Access (10 points)

For your chosen scenario, you'll need to set up API access. Here's how to get free API keys:

### For Weather Data (Scenario A):
1. **OpenWeatherMap:**
   - Go to openweathermap.org/api
   - Sign up for free account
   - Get API key (1,000 calls/day free)

2. **WeatherAPI:**
   - Go to weatherapi.com
   - Sign up for free account  
   - Get API key (1 million calls/month free)

### For News Data (Scenario B):
1. **NewsAPI:**
   - Go to newsapi.org
   - Sign up for free account
   - Get API key (1,000 requests/day free)

2. **Reddit API:**
   - Create Reddit account
   - Go to reddit.com/prefs/apps
   - Create new app, get client ID and secret

### For GitHub Data (Scenario C):
1. **GitHub API:**
   - Create GitHub account (if you don't have one)
   - Go to github.com/settings/tokens
   - Generate personal access token
   - No payment required, generous rate limits

### For Economic Data (Scenario D):
1. **Alpha Vantage:**
   - Go to alphavantage.co/support/#api-key
   - Get free API key (500 requests/day)

2. **FRED API:**
   - Go to fred.stlouisfed.org/docs/api/api_key.html
   - Request free API key

### API Key Security Best Practices:

```python
# NEVER put API keys directly in your code!
# BAD - Don't do this:
api_key = "your-secret-key-123"

# GOOD - Use environment variables:
import os
api_key = os.getenv('WEATHER_API_KEY')

# Or use a config file:
import json
with open('config.json', 'r') as f:
    config = json.load(f)
api_key = config['weather_api_key']
```

Create a `.env` file:
```
WEATHER_API_KEY=your_actual_key_here
NEWS_API_KEY=your_news_key_here
```

**Deliverable 3:**
- Screenshot showing successful API key creation
- Test script that successfully calls your chosen API
- Config file template (with fake keys as examples)

---

## Part 4: Build Your AI Data Collection Agent (35 points)

Now for the main event! Your AI agent should be a Python class that intelligently manages data collection.

### Agent Requirements:

Your agent must include these components:

#### 1. Configuration Management
```python
class DataCollectionAgent:
    def __init__(self, config_file):
        """Initialize agent with configuration from your DMP"""
        self.config = self.load_config(config_file)
        self.data_store = []
        self.collection_stats = {
            'total_requests': 0,
            'successful_requests': 0,
            'failed_requests': 0,
            'data_quality_score': 0
        }
        
    def load_config(self, config_file):
        """Load collection parameters from DMP"""
        # Load your data management plan parameters
        pass
```

#### 2. Intelligent Collection Strategy
```python
def collect_data(self):
    """Main collection loop with adaptive strategy"""
    while not self.collection_complete():
        # Assess current data quality
        quality_score = self.assess_data_quality()
        
        # Adapt strategy based on success rate
        if self.get_success_rate() < 0.8:
            self.adjust_strategy()
        
        # Make API calls with rate limiting
        data = self.make_api_request()
        
        if data:
            # Process and validate data
            processed_data = self.process_data(data)
            if self.validate_data(processed_data):
                self.store_data(processed_data)
        
        # Respectful delay
        self.respectful_delay()
```

#### 3. Data Quality Assessment
```python
def assess_data_quality(self):
    """Evaluate the quality of collected data"""
    if not self.data_store:
        return 0
    
    quality_metrics = {
        'completeness': self.check_completeness(),
        'accuracy': self.check_accuracy(), 
        'consistency': self.check_consistency(),
        'timeliness': self.check_timeliness()
    }
    
    # Calculate overall quality score
    return sum(quality_metrics.values()) / len(quality_metrics)
```

#### 4. Adaptive Strategy
```python
def adjust_strategy(self):
    """Modify collection approach based on performance"""
    success_rate = self.get_success_rate()
    
    if success_rate < 0.5:
        # Increase delays, try alternative APIs
        self.delay_multiplier *= 2
        self.try_fallback_api()
    elif success_rate > 0.9:
        # Can be more aggressive
        self.delay_multiplier *= 0.8
    
    # Log strategy changes
    self.log_strategy_change()
```

#### 5. Respectful Collection
```python
def respectful_delay(self):
    """Implement respectful rate limiting"""
    base_delay = self.config.get('base_delay', 1.0)
    delay = base_delay * self.delay_multiplier
    
    # Add random jitter to avoid thundering herd
    jitter = random.uniform(0.5, 1.5)
    time.sleep(delay * jitter)
    
def check_rate_limits(self):
    """Monitor and respect API rate limits"""
    # Check if we're approaching limits
    # Adjust collection speed accordingly
    pass
```

### Example Agent Structure:

```python
import requests
import time
import json
import random
from datetime import datetime
import logging

class WeatherDataAgent:
    def __init__(self, config_file):
        self.config = self.load_config(config_file)
        self.setup_logging()
        self.data_store = []
        self.collection_stats = {
            'start_time': datetime.now(),
            'total_requests': 0,
            'successful_requests': 0,
            'failed_requests': 0,
            'apis_used': set(),
            'quality_scores': []
        }
        self.delay_multiplier = 1.0
        
    def setup_logging(self):
        """Setup logging for the agent"""
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(levelname)s - %(message)s',
            handlers=[
                logging.FileHandler('data_collection.log'),
                logging.StreamHandler()
            ]
        )
        self.logger = logging.getLogger(__name__)
    
    def run_collection(self):
        """Main execution method"""
        self.logger.info("Starting data collection agent")
        
        try:
            while not self.collection_complete():
                data = self.collect_batch()
                if data:
                    self.process_and_store(data)
                
                # Assess and adapt
                self.assess_performance()
                self.respectful_delay()
                
        except Exception as e:
            self.logger.error(f"Collection failed: {e}")
        finally:
            self.generate_final_report()
    
    # Add all your methods here...
```

**Deliverable 4:**
- Complete Python agent class (well-commented)
- Configuration file based on your DMP
- Test results showing the agent successfully collecting data
- Log file showing respectful collection practices

---

## Part 5: Documentation and Quality Assurance (20 points)

Your agent should automatically generate comprehensive documentation.

### Required Documentation Features:

#### 1. Automated Metadata Generation
```python
def generate_metadata(self):
    """Create comprehensive metadata for collected dataset"""
    metadata = {
        'collection_info': {
            'collection_date': datetime.now().isoformat(),
            'agent_version': '1.0',
            'collector': 'YourName',
            'total_records': len(self.data_store)
        },
        'data_sources': self.get_sources_used(),
        'quality_metrics': self.calculate_final_quality_metrics(),
        'processing_history': self.get_processing_log(),
        'variables': self.generate_data_dictionary()
    }
    
    with open('dataset_metadata.json', 'w') as f:
        json.dump(metadata, f, indent=2)
```

#### 2. Quality Report Generation
```python
def generate_quality_report(self):
    """Create detailed quality assessment report"""
    report = {
        'summary': {
            'total_records': len(self.data_store),
            'collection_success_rate': self.get_success_rate(),
            'overall_quality_score': self.get_overall_quality_score()
        },
        'completeness_analysis': self.analyze_completeness(),
        'data_distribution': self.analyze_distribution(),
        'anomaly_detection': self.detect_anomalies(),
        'recommendations': self.generate_recommendations()
    }
    
    # Save as both JSON and human-readable format
    with open('quality_report.json', 'w') as f:
        json.dump(report, f, indent=2)
    
    self.create_readable_report(report)
```

#### 3. Collection Summary
Your agent should produce a final summary including:
- Total data points collected
- Success/failure rates by API
- Quality metrics and trends
- Any issues encountered
- Recommendations for future collection

**Deliverable 5:**
- Automated metadata file
- Quality assessment report  
- Collection summary document
- Screenshots of your agent running

---

## Submission Requirements

Submit a ZIP file containing:

### File Structure:
```
your_name_ai_agent_assignment/
├── README.md                    # Project overview and instructions
├── data_management_plan.pdf     # Your mini-DMP from Part 1
├── agent/
│   ├── data_collection_agent.py # Your main agent class
│   ├── config.json             # Configuration file
│   ├── requirements.txt        # Python dependencies
│   └── tests/
│       └── test_agent.py       # Basic tests
├── data/
│   ├── raw/                    # Raw collected data
│   ├── processed/              # Cleaned data
│   └── metadata/               # Generated documentation
├── logs/
│   └── collection.log          # Agent execution logs
├── reports/
│   ├── quality_report.html     # Human-readable quality report
│   └── collection_summary.pdf  # Final summary
└── demo/
    ├── api_exercises.py        # Your Part 2 exercises
    └── demo_screenshots/       # Screenshots of agent running
```

### Code Quality Requirements:
- **Documentation**: Every function must have docstrings
- **Error Handling**: Proper try/catch blocks and logging
- **Rate Limiting**: Respectful delays and API limit monitoring
- **Configuration**: No hardcoded values, use config files
- **Testing**: Basic unit tests for key functions

### Written Components:
1. **README.md** (2-3 pages): Project overview, setup instructions, usage guide
2. **Mini Data Management Plan** (1-2 pages): From Part 1
3. **Quality Report** (auto-generated): Data quality analysis
4. **Collection Summary** (1 page): Results and lessons learned

---

## Grading Rubric

| Component | Excellent (A) | Good (B) | Satisfactory (C) | Needs Improvement (D/F) |
|-----------|---------------|----------|------------------|-------------------------|
| **Mini DMP (20 pts)** | Comprehensive, specific, well-researched | Good coverage, minor gaps | Basic requirements met | Incomplete or generic |
| **API Fundamentals (15 pts)** | Perfect execution, creative extensions | All exercises working | Basic requirements met | Some exercises not working |
| **API Setup (10 pts)** | All keys working, excellent security | Keys working, good security | Keys working, basic security | Keys not working |
| **AI Agent (35 pts)** | Sophisticated, adaptive, well-designed | Good functionality, some AI features | Basic collection working | Agent not functional |
| **Documentation (20 pts)** | Comprehensive, auto-generated, professional | Good documentation, mostly complete | Basic documentation | Poor or missing documentation |

### Bonus Points (up to 10 pts):
- Creative features beyond requirements
- Exceptional code quality and documentation
- Innovative approaches to data quality or collection strategies
- Help other students on class forum

---

## Resources and Help

### Getting Started Resources:
- **Python Requests Tutorial**: [Real Python Requests Guide](https://realpython.com/python-requests/)
- **API Testing**: Use [Postman](https://www.postman.com/) or [HTTPie](https://httpie.io/) to test APIs
- **JSON Handling**: [Working with JSON in Python](https://realpython.com/python-json/)

### Free APIs for Practice:
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) - Fake REST API for testing
- [Dog CEO API](https://dog.ceo/dog-api/) - Dog images API
- [Rest Countries](https://restcountries.com/) - Country information
- [Public APIs List](https://github.com/public-apis/public-apis) - Huge list of free APIs

### Code Examples Repository:
Check the course repository for:
- Agent templates for each scenario
- Configuration file examples
- Common error handling patterns
- Rate limiting implementations

### Getting Help:
- **Office Hours**: [Insert times]
- **Class Forum**: Post questions and help classmates
- **Study Groups**: Form groups to tackle challenges together
- **Technical Issues**: Email TA with specific error messages

### Common Challenges and Solutions:

**Challenge**: "My API calls are failing"
**Solution**: Check your API key, read the documentation, verify the endpoint URL

**Challenge**: "I'm getting rate limited"  
**Solution**: Add longer delays, respect the API's rate limits, consider using multiple APIs

**Challenge**: "My agent is too slow"
**Solution**: Implement concurrent requests (carefully!), cache results, optimize your processing

**Challenge**: "Data quality is poor"
**Solution**: Add validation checks, implement retry logic, use multiple data sources

---

## Extension Ideas (Optional)

If you finish early or want to go beyond the requirements:

### Advanced Features:
- **Multi-threading**: Collect from multiple sources simultaneously
- **Machine Learning**: Use ML to predict optimal collection times
- **Real-time Monitoring**: Create a dashboard showing collection progress  
- **Data Visualization**: Generate charts showing data quality trends
- **Alerting System**: Send notifications when collection issues occur

### Integration Opportunities:
- **Database Storage**: Store data in SQLite or PostgreSQL
- **Cloud Deployment**: Deploy your agent to run automatically
- **API Creation**: Turn your agent into an API that others can use
- **Containerization**: Package your agent with Docker

---

## Learning Outcomes Assessment

By completing this assignment, you will have demonstrated:

✅ **Technical Skills:**
- API integration and authentication
- Error handling and logging
- Data validation and quality assessment
- File I/O and data serialization

✅ **Data Management:**
- Creating and following a data management plan
- Implementing respectful collection practices
- Generating comprehensive documentation
- Quality assurance and validation

✅ **Problem Solving:**
- Debugging API integration issues
- Adapting strategies based on performance
- Handling edge cases and errors
- Optimizing collection efficiency

✅ **Professional Practices:**
- Code documentation and organization
- Version control and reproducibility
- Ethical data collection
- Collaborative development practices

---

**Good luck building your AI data collection agent! Remember, the goal is not just to collect data, but to do so responsibly, efficiently, and with proper documentation. Your future self (and your research collaborators) will thank you for the time invested in good practices.**

---

*If you have any questions about this assignment, please don't hesitate to ask during office hours or on the class forum. We're here to help you succeed!*
