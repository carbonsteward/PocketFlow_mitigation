# Daily Automation Architecture

## Overall Daily Workflow
```
🕐 00:00 UTC Daily Trigger
    ↓
📚 Multi-Source Ingestion (BatchFlow)
    ↓  
⚡ Validation Pipeline (AsyncParallelBatchFlow)
    ↓
🎯 GRI/SBTi Sector Classification (Flow)
    ↓
🔄 Data Deduplication & Merging
    ↓
📊 Metadata Enrichment
    ↓
📁 GitHub Archive Update
    ↓
📧 Daily Intelligence Report
```

## GitHub Actions Workflow Structure

### Primary Workflow: `.github/workflows/daily-climate-intelligence.yml`
```yaml
name: Daily Climate Intelligence Scraper
on:
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight UTC
  workflow_dispatch:    # Manual trigger option

jobs:
  climate-intelligence:
    runs-on: ubuntu-latest
    timeout-minutes: 180  # 3 hour timeout
    
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v4
      
    - name: Setup Python Environment
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
        
    - name: Install Dependencies
      run: |
        pip install -r requirements.txt
        
    - name: Run Multi-Source Ingestion
      env:
        ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        SERPAPI_API_KEY: ${{ secrets.SERPAPI_API_KEY }}
      run: python flows/daily_ingestion_flow.py
      
    - name: Run Validation Pipeline
      run: python flows/daily_validation_flow.py
      
    - name: Run Sector Classification
      run: python flows/daily_classification_flow.py
      
    - name: Update Archive
      run: python archive/update_daily_archive.py
      
    - name: Commit and Push Changes
      run: |
        git config --local user.email "climate-intelligence@pocketflow.dev"
        git config --local user.name "Climate Intelligence Bot"
        git add data/
        git commit -m "Daily climate intelligence update: $(date +%Y-%m-%d)" || exit 0
        git push
        
    - name: Generate Daily Report
      run: python reports/generate_daily_report.py
```

## Data Storage Structure in GitHub Repository

### Repository Structure: `climate-intelligence-archive/`
```
climate-intelligence-archive/
├── data/
│   ├── technologies/
│   │   ├── 2024/
│   │   │   ├── 01/
│   │   │   │   ├── daily_discoveries_2024-01-15.json
│   │   │   │   ├── daily_discoveries_2024-01-16.json
│   │   │   │   └── ...
│   │   │   └── 02/
│   │   └── master_technology_database.json
│   ├── validation_scores/
│   │   ├── daily_scores_2024-01-15.json
│   │   └── ...
│   ├── sector_mappings/
│   │   ├── gri_mappings_2024-01-15.json
│   │   ├── sbti_mappings_2024-01-15.json
│   │   └── ...
│   └── research_sources/
│       ├── papers_2024-01-15.json
│       ├── patents_2024-01-15.json
│       └── institutional_2024-01-15.json
├── reports/
│   ├── daily/
│   │   ├── climate_intelligence_2024-01-15.md
│   │   └── ...
│   ├── weekly/
│   └── monthly/
├── analytics/
│   ├── trending_technologies.json
│   ├── sector_analysis.json
│   └── research_momentum.json
└── api/
    └── search_interface.html
```

## Data Update Logic

### New Technology Discovery Flow
```python
def process_daily_discoveries():
    """
    Handle new technology discoveries and updates to existing entries
    """
    # 1. Load existing master database
    master_db = load_master_database()
    
    # 2. Process daily scraped data
    daily_data = process_daily_scraping_results()
    
    # 3. Deduplication logic
    for new_tech in daily_data:
        existing_match = find_similar_technology(new_tech, master_db)
        
        if existing_match:
            # Update existing entry with new metadata
            updated_tech = merge_technology_data(existing_match, new_tech)
            master_db.update_entry(updated_tech)
            log_update(existing_match.technology_id, new_tech)
        else:
            # Add new technology entry
            new_tech_id = generate_technology_id(new_tech)
            master_db.add_entry(new_tech_id, new_tech)
            log_new_discovery(new_tech_id, new_tech)
    
    # 4. Save updated database
    save_master_database(master_db)
    
    # 5. Generate daily diff report
    generate_daily_changes_report()
```

### Deduplication Strategy
```python
def find_similar_technology(new_tech, master_db):
    """
    Identify if new technology matches existing entry
    """
    # 1. Exact name matching (normalized)
    normalized_name = normalize_technology_name(new_tech.name)
    exact_match = master_db.find_by_normalized_name(normalized_name)
    if exact_match:
        return exact_match
    
    # 2. Semantic similarity matching
    embedding = get_technology_embedding(new_tech.description)
    similar_entries = master_db.find_by_embedding_similarity(embedding, threshold=0.85)
    
    # 3. Multi-factor matching
    for candidate in similar_entries:
        similarity_score = calculate_technology_similarity(
            new_tech, candidate,
            factors=['name', 'description', 'sector', 'research_papers']
        )
        if similarity_score > 0.90:
            return candidate
    
    return None
```

## Public API for Archive Access

### Search Interface: `api/search_interface.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title>Climate Intelligence Archive Search</title>
</head>
<body>
    <h1>🌍 Climate Intelligence Archive</h1>
    
    <div class="search-container">
        <input type="text" id="search-input" placeholder="Search technologies (e.g., 'ammonia fuel cells', 'cement pozzolan')">
        <button onclick="searchTechnologies()">Search</button>
    </div>
    
    <div class="filters">
        <select id="sector-filter">
            <option value="">All Sectors</option>
            <option value="maritime">Maritime</option>
            <option value="cement">Cement</option>
            <option value="power">Power</option>
        </select>
        
        <select id="priority-filter">
            <option value="">All Priorities</option>
            <option value="high">High Priority</option>
            <option value="medium">Medium Priority</option>
            <option value="research_watch">Research Watch</option>
        </select>
    </div>
    
    <div id="results-container"></div>
    
    <script>
        function searchTechnologies() {
            // Implementation for searching the JSON database
            // Returns filtered results with metadata
        }
    </script>
</body>
</html>
```

## Monitoring & Alerts

### Daily Success Metrics
- Number of new papers processed
- Number of new technologies discovered  
- Number of existing entries updated
- Validation pipeline success rate
- Data quality scores
- Processing time metrics

### Alert Conditions
- Pipeline failures
- Unusually high/low discovery rates
- Data quality degradation
- API rate limiting issues
- Storage capacity warnings

### Daily Report Template
```markdown
# Climate Intelligence Daily Report - {DATE}

## 📊 Discovery Summary
- **New Technologies Identified**: {count}
- **Existing Entries Updated**: {count}
- **Total Technologies Tracked**: {count}
- **Papers Processed**: {count}
- **Patents Analyzed**: {count}

## 🔥 High Priority Discoveries
{list of high-priority technologies discovered today}

## 📈 Trending Technologies
{technologies showing increased research momentum}

## 🎯 Sector Analysis
{breakdown by GRI/SBTi sectors}

## ⚠️ Quality Alerts
{any data quality issues or conflicting findings}
```

## Backup & Recovery Strategy

### Data Redundancy
- Primary: GitHub repository (version controlled)
- Secondary: Cloud storage backup (daily snapshots)
- Tertiary: Local development environment syncing

### Recovery Procedures
- Automated daily backups to cloud storage
- Git history provides complete audit trail
- Database reconstruction capabilities from daily JSON files
- Rollback procedures for data corruption incidents