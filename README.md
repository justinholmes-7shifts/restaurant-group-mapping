# Restaurant Group Mapping

## Overview
A tool to identify sister restaurants and restaurant groups by analyzing their websites and online presence. Given a list of restaurants in a city, this project helps discover which restaurants belong to the same parent company or restaurant group.

## Purpose
- **Discover restaurant groups:** Identify which restaurants are owned/operated by the same company
- **Map relationships:** Build a network of sister restaurants across cities
- **Market intelligence:** Understand restaurant group portfolios and expansion patterns
- **Competitive analysis:** See which groups operate in your target markets
- **Sales targeting:** Identify multi-location opportunities for B2B products

## How It Works

1. **Input:** List of restaurants (name, city, optional URL)
2. **Analysis:** Claude Code searches for and analyzes restaurant websites
3. **Mapping:** Identifies common ownership through:
   - Website domains and URL patterns
   - Parent company mentions
   - Shared branding and design
   - Contact information patterns
   - Social media profiles
   - About/locations pages
4. **Output:** Grouped restaurants by parent company/restaurant group

## Project Structure

```
restaurant-group-mapping/
├── README.md                          # This file
├── WORKFLOW.md                        # Step-by-step analysis guide
├── data/
│   ├── input/                         # Your restaurant lists
│   │   ├── template.csv               # CSV template
│   │   └── [city]-restaurants.csv     # Your data files
│   └── processed/                     # Cleaned/enriched data
├── results/
│   ├── [city]-restaurant-groups.json  # Grouped results
│   └── [city]-analysis-report.md      # Human-readable report
└── prompts/
    └── mapping-prompt-template.md     # Analysis prompt templates
```

## Quick Start

### 1. Prepare Your Restaurant List

Create a CSV file in `data/input/` with your restaurants:

```csv
restaurant_name,city,state,url,notes
Joe's Pizza,Toronto,ON,,
Mario's Italian,Toronto,ON,https://mariositalian.com,
The Keg,Toronto,ON,,Part of Keg Restaurants chain
```

**Required fields:**
- `restaurant_name`: Name of the restaurant
- `city`: City where located

**Optional fields:**
- `state`: State/province
- `url`: Restaurant website (if known)
- `notes`: Any additional context

### 2. Run the Analysis

Open Claude Code and use the prompt template from `prompts/mapping-prompt-template.md`, or use this quick version:

```
I have a list of restaurants in [city]. For each restaurant:
1. Search for their website if not provided
2. Analyze the website to identify parent company/restaurant group
3. Look for sister restaurants mentioned on the site
4. Group restaurants by common ownership

Input file: data/input/[your-file].csv

Create:
- results/[city]-restaurant-groups.json (structured data)
- results/[city]-analysis-report.md (human-readable report)
```

### 3. Review Results

Check the generated files in `results/`:
- **JSON:** Structured data for programmatic use
- **Report:** Human-readable analysis with evidence

## Use Cases

### Sales & Business Development
- Identify multi-location restaurant groups for B2B outreach
- Find expansion-minded groups with growth potential
- Map competitive landscape in target markets

### Market Research
- Understand restaurant group consolidation in a region
- Track which groups are expanding into new cities
- Identify independent vs chain restaurants

### Competitive Intelligence
- See which restaurant groups dominate a market
- Identify emerging regional players
- Understand group portfolio composition (QSR, casual, fine dining)

### Real Estate & Site Selection
- Identify restaurant groups with site selection teams
- Find groups with multi-unit development plans
- Map existing footprints of restaurant groups

## Example Output

### JSON Structure
```json
{
  "analysis_date": "2024-11-25",
  "city": "Toronto",
  "restaurant_groups": [
    {
      "group_name": "ABC Restaurant Group",
      "parent_company": "ABC Hospitality Inc.",
      "website": "https://abcrestaurants.com",
      "restaurants": [
        {
          "name": "Restaurant A",
          "url": "https://restauranta.com",
          "evidence": "Footer shows 'Part of ABC Restaurant Group'"
        },
        {
          "name": "Restaurant B",
          "url": "https://restaurantb.com",
          "evidence": "Locations page lists both as ABC properties"
        }
      ],
      "sister_restaurants_other_cities": [
        "Restaurant C (Vancouver)",
        "Restaurant D (Montreal)"
      ]
    }
  ],
  "independent_restaurants": [
    {
      "name": "Joe's Pizza",
      "url": "https://joespizza.com",
      "evidence": "Single location, no parent company mentioned"
    }
  ]
}
```

### Report Format
```markdown
# Toronto Restaurant Group Analysis

## Summary
- Total restaurants analyzed: 25
- Restaurant groups identified: 8
- Independent restaurants: 12
- Unable to determine: 5

## Restaurant Groups

### ABC Restaurant Group
**Parent Company:** ABC Hospitality Inc.
**Website:** https://abcrestaurants.com
**Restaurants in Toronto:**
1. Restaurant A - https://restauranta.com
2. Restaurant B - https://restaurantb.com

**Evidence:**
- Both websites have identical footer with "© ABC Restaurant Group"
- Locations page on restauranta.com lists both properties
- Shared contact email domain: @abcrestaurants.com

**Other Locations:**
- Vancouver: Restaurant C
- Montreal: Restaurant D
```

## Tips for Better Results

### Provide Context
- Include any URLs you already know
- Note obvious chains (e.g., "Starbucks")
- Mention if you know restaurants are related

### Search Strategies
- Look for "locations" or "our restaurants" pages
- Check website footers for parent company info
- Search for "about us" sections
- Look at domain registration patterns
- Check social media profiles for company links

### Handling Ambiguity
- Some restaurants may have franchise relationships
- Licensing vs ownership can be unclear
- Management companies vs ownership groups
- Multi-brand operators

## Common Patterns

### URL Patterns
- `restauranta.group.com` and `restaurantb.group.com` → Same group
- Shared domain but different subdomains → Likely same group
- Completely different domains → Need further investigation

### Website Evidence
- Shared footer text
- Common "About" page mentioning parent company
- Locations page listing multiple concepts
- Press releases mentioning ownership
- Shared contact information

### Social Media
- Instagram/Facebook pages linking to parent company
- Shared management team across accounts
- Cross-promotion between restaurant accounts

## Limitations

**What This Can Find:**
- Restaurants that publicly acknowledge group ownership
- Sister restaurants listed on company websites
- Clear branding and URL relationships

**What This Might Miss:**
- Private holding companies not mentioned online
- Complex franchise relationships
- Recently acquired restaurants with separate branding
- Silent ownership structures
- International parent companies

## Advanced Features

### Multi-City Analysis
Run analysis across multiple cities to map regional/national groups:
```
Analyze restaurant lists from data/input/ for:
- Toronto, Vancouver, Montreal

Identify restaurant groups operating in multiple cities.
Create a cross-city relationship map.
```

### Competitive Landscape Mapping
```
From the identified restaurant groups:
1. Categorize by segment (QSR, Fast Casual, Casual Dining, Fine Dining)
2. Count locations per group
3. Identify which groups are expanding
4. Map market share by group
```

### Timeline Tracking
Track changes over time:
- New restaurants added to groups
- Acquisitions and consolidation
- Market entry by new groups

## Data Privacy

**Important:**
- Only use publicly available information
- Respect website terms of service
- Don't scrape excessive data
- Focus on business intelligence, not personal data

## Contributing

### Add Your City
1. Create `data/input/[city]-restaurants.csv`
2. Run analysis
3. Commit results to `results/`
4. Update this README with findings

### Improve Prompts
- Share better search strategies in `prompts/`
- Document new patterns you discover
- Add examples of successful mappings

## Version History
- v1.0 - Initial project setup
- Focus: URL-based restaurant group identification

---

## Getting Started

Ready to map your restaurant landscape?

1. Create your restaurant list in `data/input/`
2. Check `WORKFLOW.md` for detailed process
3. Use prompts from `prompts/` directory
4. Share your findings!

Happy mapping! 🗺️🍽️
