# Restaurant Group Mapping Workflow

## Step-by-Step Process

---

## Phase 1: Data Preparation (5-10 minutes)

### 1.1 Create Your Restaurant List

**Option A: Start from Template**
```bash
cp data/input/template.csv data/input/[city]-restaurants.csv
```

Edit the file with your restaurants:
```csv
restaurant_name,city,state,url,notes
Restaurant A,Toronto,ON,,
Restaurant B,Toronto,ON,https://restaurantb.com,
Restaurant C,Toronto,ON,,Known to be part of XYZ Group
```

**Option B: Create from Scratch**

Required fields:
- `restaurant_name`: Full restaurant name
- `city`: City location

Optional but helpful:
- `state`: State/province code
- `url`: Website URL if known
- `notes`: Any context (chain, independent, suspected group, etc.)

### 1.2 Quality Check Your Data
- Remove duplicates
- Standardize restaurant names
- Include as many URLs as you have
- Note obvious chains vs independents

---

## Phase 2: Website Discovery (10-20 minutes)

If you don't have URLs for all restaurants, use Claude Code to find them.

### 2.1 Search for Missing URLs

**Prompt:**
```
I have a list of restaurants in [city] without URLs in data/input/[file].csv.

For each restaurant without a URL:
1. Search for "[restaurant name] [city]" to find their website
2. Verify it's the correct location (same city)
3. Add the URL to the CSV

Update the CSV file with found URLs.
Note any restaurants where you couldn't find a website.
```

### 2.2 Verify URLs
- Check that URLs actually work
- Confirm they're the right location (not a different city)
- Note if it's a single location vs multi-location site

---

## Phase 3: Restaurant Group Analysis (20-40 minutes)

### 3.1 Run the Mapping Analysis

Use the prompt template from `prompts/mapping-prompt-template.md` or customize this:

```
I have a CSV file of restaurants in [city] at data/input/[city]-restaurants.csv.

For each restaurant with a URL:
1. Visit the website and analyze it
2. Look for evidence of parent company or restaurant group:
   - Footer text mentioning parent company
   - "About Us" page with company info
   - "Locations" or "Our Restaurants" pages showing sister concepts
   - Contact information patterns (shared email domains, phone numbers)
   - Copyright statements
   - Press releases or news sections
3. Search for sister restaurants (other concepts by same group)
4. Note the type of evidence found

For restaurants without URLs that you found:
- Search for "[restaurant name] parent company"
- Search for "[restaurant name] restaurant group"
- Check LinkedIn company pages

Group restaurants by:
- Restaurant Group (if identifiable)
- Parent Company (if different from group name)
- Independent (if no group affiliation found)
- Unknown (if unable to determine)

Create two output files:

1. results/[city]-restaurant-groups.json
Format: {
  "analysis_date": "YYYY-MM-DD",
  "city": "[city]",
  "summary": {
    "total_restaurants": 0,
    "restaurant_groups": 0,
    "independent_restaurants": 0,
    "unknown": 0
  },
  "restaurant_groups": [
    {
      "group_name": "",
      "parent_company": "",
      "website": "",
      "restaurants_in_city": [
        {
          "name": "",
          "url": "",
          "evidence": ""
        }
      ],
      "sister_restaurants_other_cities": []
    }
  ],
  "independent_restaurants": [
    {
      "name": "",
      "url": "",
      "evidence": ""
    }
  ],
  "unknown": [
    {
      "name": "",
      "reason": ""
    }
  ]
}

2. results/[city]-analysis-report.md
Human-readable report with:
- Executive summary
- List of restaurant groups with details
- Independent restaurants
- Evidence for each classification
- Sister restaurants discovered
- Recommendations for further research
```

### 3.2 What to Look For

**Strong Evidence of Group Ownership:**
- Website footer: "© 2024 ABC Restaurant Group"
- About page: "We operate 5 concepts across Canada..."
- Locations page showing multiple restaurant brands
- Shared domain pattern (brand1.group.com, brand2.group.com)
- Press releases about the group
- Parent company mentioned in contact/legal pages

**Moderate Evidence:**
- Similar website design across restaurants
- Shared social media profiles or cross-promotion
- Same contact email domain
- Similar menu structures or branding
- Common leadership team mentioned

**Weak Evidence:**
- Similar restaurant names (could be coincidence)
- Same neighborhood (could be unrelated)
- Similar cuisine type (not evidence of ownership)

### 3.3 Validation Steps

For identified groups:
```
Search for "[group name] restaurants" to verify
Check if the group's main website lists all restaurants found
Look for press releases or news about the group
Verify on LinkedIn or company databases
```

---

## Phase 4: Deep Dive on Interesting Groups (Optional, 15-30 minutes)

### 4.1 Expand the Map

For major restaurant groups found:
```
For [Restaurant Group Name]:
1. Search for all locations across Canada/US
2. Find their full portfolio of brands
3. Identify expansion patterns (which cities they're entering)
4. Look for recent acquisitions or new concepts
5. Note their target segments (QSR, casual, fine dining)

Add to results/[city]-restaurant-groups.json under each group.
```

### 4.2 Competitive Landscape

```
Analyze the restaurant groups found in [city]:
1. Rank by number of restaurants in city
2. Categorize by segment (QSR, Fast Casual, Casual, Fine Dining)
3. Identify which groups have multi-city presence
4. Note which groups are expanding vs established

Create results/[city]-competitive-landscape.md
```

---

## Phase 5: Results Review & Refinement (10-15 minutes)

### 5.1 Quality Check

Review the generated files:
- [ ] All restaurants from input are accounted for
- [ ] Evidence is specific and cited (not generic)
- [ ] URLs are accurate and working
- [ ] Group names are consistent
- [ ] Sister restaurants are verified (not speculation)

### 5.2 Refine Uncertain Cases

For restaurants marked as "unknown":
```
For these restaurants that were unclear:
[list restaurants]

Try alternative search strategies:
- Search for LinkedIn company pages
- Search for news articles mentioning the restaurant
- Check business registries (if available)
- Look for trademark or corporate filings
- Search Instagram/Facebook for "about" information

Update results files with any new findings.
```

### 5.3 Document Assumptions

Add notes to results about:
- Restaurants where evidence was weak but classification made
- Potential franchise relationships (vs corporate ownership)
- Cases where further research would be valuable

---

## Phase 6: Share & Commit (5 minutes)

### 6.1 Final Review
- Check that JSON is valid (no syntax errors)
- Ensure report is readable and well-formatted
- Verify all file paths are correct

### 6.2 Commit to Repository

```bash
git add data/input/[city]-restaurants.csv
git add results/[city]-restaurant-groups.json
git add results/[city]-analysis-report.md
git commit -m "Add restaurant group mapping for [city]"
git push
```

---

## Common Scenarios

### Scenario 1: Restaurant with Multiple Locations (Same Brand)

**Example:** "Pizzeria ABC" has 5 locations in Toronto

**Approach:**
- This is NOT a restaurant group (it's one brand)
- Note: "Multi-location independent restaurant (5 locations in Toronto)"
- Look for: Do they operate OTHER restaurant brands?

### Scenario 2: Franchise Relationship

**Example:** Website says "Franchised by XYZ Corp"

**Approach:**
- Note the franchise relationship
- Group by franchisor if that's the analysis goal
- Or mark as "Independent Franchisee" if focusing on ownership
- Document the ambiguity in evidence field

### Scenario 3: Shared Domain, Different Brands

**Example:**
- restauranta.abcgroup.com
- restaurantb.abcgroup.com

**Approach:**
- Strong evidence of group ownership
- Group them under "ABC Group"
- Evidence: "Shared domain structure (*.abcgroup.com)"

### Scenario 4: No Website Found

**Example:** Restaurant has no website, just Google listing

**Approach:**
1. Search for social media profiles
2. Check Google Maps info (sometimes mentions parent company)
3. Search for news articles or reviews mentioning ownership
4. If truly nothing found, mark as "Unknown - No web presence"

### Scenario 5: Conflicting Information

**Example:** Website says one thing, news article says another

**Approach:**
- Prioritize more recent information
- Note the discrepancy in evidence field
- Flag for manual verification
- Example: "Website lists as ABC Group, but 2023 news article mentions sale to XYZ Corp - needs verification"

---

## Tips for Efficiency

### Use Claude Code Effectively
- Process restaurants in batches (10-15 at a time)
- Provide context about the city/market
- Ask for specific evidence citations
- Request structured output from the start

### Search Strategies
**For finding websites:**
- "[restaurant name] [city] official website"
- "[restaurant name] [city] menu" (often leads to site)

**For finding parent companies:**
- "[restaurant name] parent company"
- "[restaurant name] ownership"
- "[restaurant name] restaurant group"
- site:[restauranturl.com] "about" OR "company" OR "group"

**For finding sister restaurants:**
- site:[restauranturl.com] "locations" OR "restaurants"
- "[parent company name] restaurant brands"

### Time-Saving Tricks
1. **Focus on unclear cases:** If it's obviously a major chain (Starbucks, McDonald's), note it and move on
2. **Batch similar restaurants:** Process all suspected independents together
3. **Use pattern recognition:** If you find one restaurant in a group, search for the group's full portfolio
4. **Document as you go:** Don't save all writing for the end

---

## Validation Checklist

Before considering analysis complete:

- [ ] Every restaurant has a classification (group/independent/unknown)
- [ ] All group assignments have specific evidence cited
- [ ] Sister restaurants are verified (not assumed)
- [ ] URLs are tested and working
- [ ] JSON structure is valid
- [ ] Report is readable by non-technical stakeholders
- [ ] Ambiguous cases are documented
- [ ] Summary statistics are accurate

---

## Output Quality Examples

### Good Evidence Citation
✅ "Footer text: '© 2024 ABC Hospitality Group. Brands: Restaurant A, Restaurant B, Restaurant C'"

❌ "Appears to be part of ABC Group"

### Good Group Identification
✅ "ABC Restaurant Group (parent: ABC Hospitality Inc.)"
- Website: https://abchospitality.com
- Evidence: Company website lists all 3 brands

❌ "ABC Group" (no URL, no evidence, no details)

### Good Sister Restaurant List
✅ "Other locations listed on website: Vancouver (2), Montreal (1), Calgary (1)"

❌ "Has locations in other cities" (not specific)

---

## What to Do Next

After completing your analysis:

1. **Share insights:** Use the report for sales targeting, market research, etc.
2. **Expand coverage:** Add more cities to compare regional presence
3. **Track changes:** Re-run analysis quarterly to track acquisitions and growth
4. **Enhance data:** Add revenue estimates, employee counts, or other metrics
5. **Build relationships:** Use findings to prioritize outreach to multi-location groups

---

## Troubleshooting

### "Claude Code can't access the website"
- Try searching for cached versions or archived pages
- Look for PDF menus or press releases that mention ownership
- Check social media profiles for company links

### "Results are inconsistent"
- Provide more specific instructions about evidence requirements
- Ask for direct quotes from websites
- Request URL citations for all claims

### "Too many 'unknown' classifications"
- Try additional search strategies in prompts
- Lower the bar for what counts as evidence
- Note which restaurants are worth manual research

### "Analysis is taking too long"
- Break into smaller batches (5-10 restaurants)
- Focus on high-priority targets first
- Skip obvious major chains
- Use parallel searches for multiple restaurants

---

## Advanced Workflows

### Multi-City Comparison
```
Analyze restaurant groups across:
- data/input/toronto-restaurants.csv
- data/input/vancouver-restaurants.csv
- data/input/montreal-restaurants.csv

Identify groups operating in multiple cities.
Create results/canada-multi-city-groups.json
```

### Acquisition Tracking
```
Compare results/[city]-restaurant-groups-2024-Q1.json
with results/[city]-restaurant-groups-2024-Q4.json

Identify:
- New restaurant groups entered market
- Acquisitions (restaurants that changed groups)
- Closures or exits

Create results/[city]-market-changes-2024.md
```

### Segment Analysis
```
Categorize all identified restaurant groups by:
- QSR (Quick Service)
- Fast Casual
- Casual Dining
- Fine Dining
- Mixed Portfolio

Create results/[city]-segment-analysis.md showing:
- Which segments have most group consolidation
- Which have most independents
- Group strategies by segment
```

---

Ready to start mapping? Begin with Phase 1! 🗺️
