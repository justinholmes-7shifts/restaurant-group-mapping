# Restaurant Group Mapping Prompt Template

## Goal
For each restaurant provided, determine:
1. Whether the restaurant belongs to a **holding or parent company** or **restaurant group**.
2. Whether the company owns or is affiliated with **sister restaurants** or **child brands** (restaurants under the same corporate group).
3. Include the **official websites** of any detected sister/child restaurants.
4. **Explain your reasoning** (how you concluded ownership and brand relationships).
5. **Cite sources for each child/sister restaurant** you list.
6. Return the result in **JSON format**.

**Use any sources you can find to suggest that there is a sister restaurant. I.E. website, LinkedIn, Instagram, TikTok, or any other public sources you can find.**

---

## Step 1 — Locate Core Pages

Search for and review the restaurant's core legal/informational pages:
- **Privacy Policy**
- **Terms of Service / Terms & Conditions**
- **About Us / Our Story**
- **Locations** page (often shows sister concepts)
- **Corporate** or **Company** page
- **Contact** page (look for parent company mentions)

Record exact URLs reviewed.

---

## Step 2 — Detect Ownership References (Primary)

Identify any explicit mention that the restaurant:
- Is **owned by**, **part of**, **operated by**, or **a subsidiary of** a holding/parent company, or
- **Owns**, **manages**, or **operates** other restaurant brands (child/sister restaurants).

If found, extract:
- **Holding company name** or **Restaurant group name**
- **Holding company website** (if available)
- **Sister/child restaurant names**
- **Official source URL** (policy page, about page, or locations page)
- **Short excerpt** confirming ownership/affiliation
- **Confidence** = High (when confirmed on official policy/legal/about page)

**Common patterns for restaurants:**
- Footer text: "© 2024 [Restaurant Group Name]"
- About page: "We operate [X] restaurants in [city/region]..."
- Locations page showing multiple restaurant brands/concepts
- Contact page: "Operated by [Parent Company Name]"
- Press/Media sections mentioning parent company

---

## Step 3 — Extended Investigation (Secondary)

If no clear ownership is found in Step 2:

1. **Check restaurant-specific pages:**
   - **Locations** or **Our Restaurants** pages (often lists sister concepts)
   - **Press/Media** or **News** sections
   - **Careers** page (may mention parent company)
   - **Franchise Information** (if applicable)

2. **Social Media Investigation:**
   - **Instagram:** Check bio, about section, and tagged locations
   - **Facebook:** Check "About" section for company info
   - **LinkedIn:** Search for the restaurant's company page
   - **TikTok:** Check profile description

3. **Web Search (if still unclear):**
   - Search: "[restaurant name] parent company"
   - Search: "[restaurant name] restaurant group"
   - Search: "[restaurant name] ownership"
   - Check business registries or corporate databases (if accessible)

4. **For each sister/child restaurant you list**, capture:
   - Restaurant **official website**
   - **Source URL** proving the affiliation (prefer the holding company's "our brands" page; otherwise a credible secondary source)

**Confidence levels:**
- **High** = verified on an official corporate/legal page (privacy policy, about page, company group page, locations page, or official registry entry)
- **Medium** = verified via credible secondary source (reputable news, industry databases, Wikipedia with citations, LinkedIn company page)
- **Low** = inferred but **not** explicitly confirmed by a trustworthy source (avoid listing unless necessary; clearly mark as Low)

---

## Step 4 — Reasoning Requirements

In the output, include a concise **reasoning** section that:
- Summarizes which pages you checked first (privacy policy, about, locations), what they state, and why that establishes ownership
- Explains any gaps and how you resolved them (e.g., found on LinkedIn or Instagram bio)
- Notes edge cases (e.g., **franchise relationship vs corporate ownership**, **management company vs ownership group**)
- Mentions if any restaurants were excluded due to insufficient verification
- Highlights how you determined the relationship (shared footer, locations page, social media cross-references, etc.)

---

## Step 5 — Handle Missing or Unclear Cases

If no ownership or related restaurant info is found after all checks:
- **holding_company:** "None detected"
- **holding_website:** "N/A"
- **sister_child_restaurants:** []
- **sister_child_restaurants_websites:** []
- **child_restaurant_sources:** {}
- **source:** "No verified reference found"
- **evidence:** "N/A"
- **reasoning:** brief note that no verified reference was found after checking privacy policy, about page, locations page, social media, and credible sources
- **confidence:** "None"

---

## Step 6 — Output Format (STRICT JSON)

Return exactly this JSON structure for each restaurant:

```json
{
  "restaurant_name": "[Restaurant Name]",
  "restaurant_website": "[Restaurant URL or 'Not Found']",
  "city": "[City]",
  "holding_company": "[Restaurant Group name or 'None detected']",
  "holding_website": "[URL or 'N/A']",
  "sister_child_restaurants": ["Restaurant A", "Restaurant B"],
  "sister_child_restaurants_websites": ["https://restauranta.com", "https://restaurantb.com"],
  "child_restaurant_sources": {
    "Restaurant A": ["https://holdinggroup.com/our-restaurants/restaurant-a", "https://restauranta.com"],
    "Restaurant B": ["https://holdinggroup.com/locations", "https://restaurantb.com"]
  },
  "source": "[Primary verified source URL (privacy/about/locations page) or 'No verified reference found']",
  "evidence": "[short excerpt from the primary source confirming ownership/affiliation or 'N/A']",
  "reasoning": "[2–6 sentence explanation of how you concluded ownership and restaurant relationships, including mention of franchise vs corporate ownership if relevant, social media findings, and any exclusions.]",
  "confidence": "[High | Medium | Low | None]"
}
```

---

## Example Output for Restaurant

```json
{
  "restaurant_name": "Restaurant A Downtown",
  "restaurant_website": "https://restauranta.com",
  "city": "Miami",
  "holding_company": "ABC Hospitality Group",
  "holding_website": "https://abchospitality.com",
  "sister_child_restaurants": [
    "Restaurant B",
    "Restaurant C",
    "The ABC Grill"
  ],
  "sister_child_restaurants_websites": [
    "https://restaurantb.com",
    "https://restaurantc.com",
    "https://theabcgrill.com"
  ],
  "child_restaurant_sources": {
    "Restaurant B": ["https://abchospitality.com/our-brands/", "https://restaurantb.com"],
    "Restaurant C": ["https://abchospitality.com/our-brands/", "https://restaurantc.com"],
    "The ABC Grill": ["https://abchospitality.com/our-brands/", "https://theabcgrill.com"]
  },
  "source": "https://restauranta.com/privacy-policy",
  "evidence": "We are ABC Hospitality Group... we operate multiple restaurant concepts including Restaurant A, Restaurant B, Restaurant C, and The ABC Grill.",
  "reasoning": "I first checked the Privacy Policy at restauranta.com/privacy-policy, which explicitly names ABC Hospitality Group as the controller and operator. The About page at abchospitality.com lists all four restaurant brands. Each sister restaurant website was verified and contains footer text '© ABC Hospitality Group.' All websites share the same contact email domain (@abchospitality.com). High confidence due to explicit mentions across official sources.",
  "confidence": "High"
}
```

---

## Example Output for Independent Restaurant

```json
{
  "restaurant_name": "Joe's Pizza",
  "restaurant_website": "https://joespizza.com",
  "city": "Miami",
  "holding_company": "None detected",
  "holding_website": "N/A",
  "sister_child_restaurants": [],
  "sister_child_restaurants_websites": [],
  "child_restaurant_sources": {},
  "source": "https://joespizza.com/about",
  "evidence": "Family-owned and operated since 1985. Single location in Miami.",
  "reasoning": "Checked privacy policy, about page, and contact information. Website indicates 'family-owned and operated' with no mention of other locations or parent company. Social media profiles (Instagram @joespizza_miami) show independent operation. No corporate affiliation found through web search or business registry check.",
  "confidence": "High"
}
```

---

## Example Output for Franchise

```json
{
  "restaurant_name": "Subway Downtown Miami",
  "restaurant_website": "https://subway.com",
  "city": "Miami",
  "holding_company": "Subway IP LLC (franchisor)",
  "holding_website": "https://subway.com",
  "sister_child_restaurants": [],
  "sister_child_restaurants_websites": [],
  "child_restaurant_sources": {},
  "source": "https://subway.com/about",
  "evidence": "Subway restaurants are independently owned and operated franchises.",
  "reasoning": "This is a franchise location. The global franchisor is Subway IP LLC, but individual franchise locations are independently owned. Since the specific franchisee of this location is not publicly available, and franchise siblings are not corporate siblings, no sister restaurants are listed. This represents a franchise relationship rather than corporate ownership.",
  "confidence": "High"
}
```

---

## Restaurant-Specific Tips

### What to Look For:
1. **Shared website domains** (e.g., restauranta.group.com, restaurantb.group.com)
2. **Footer copyright** statements mentioning parent company
3. **Locations pages** showing multiple concepts
4. **"Our Brands" or "Our Restaurants"** pages
5. **Social media cross-promotion** between sister concepts
6. **Shared contact information** (email domains, phone numbers)
7. **Press releases** about the restaurant group
8. **LinkedIn company pages** linking restaurants

### Common Restaurant Group Patterns:
- **Multi-concept operators:** One company operates different restaurant brands
- **Regional groups:** Multiple locations of different concepts in same metro area
- **Franchise vs Corporate:** Be clear about franchise relationships
- **Management vs Ownership:** Some groups manage but don't own restaurants

### Red Flags (Weak Evidence):
- Similar restaurant names (could be coincidence)
- Same neighborhood (doesn't mean same owner)
- Similar cuisine type (not evidence of ownership)
- Similar website design (could use same template company)

---

## Usage Example

### Input CSV Format:
```csv
Company Name,Location Name,Location Address
ABC Hospitality,Restaurant A Downtown,"123 Main St, Miami, FL"
Joe's Pizza,Joe's Pizza,"456 Oak Ave, Miami, FL"
```

### Prompt to Use:

```
I have a CSV file at data/input/miami-restaurants.csv with restaurant data.

For each restaurant:
1. Search for the restaurant's website if not already known
2. Follow the Restaurant Group Mapping Prompt Template in prompts/mapping-prompt-template.md
3. Analyze privacy policy, about pages, locations pages, and social media
4. Identify parent company/restaurant group and sister restaurants
5. Return results in the JSON format specified in the template

Create results/miami-restaurant-groups.json with an array of all results.

Be thorough in checking:
- Website pages (privacy, about, locations, contact)
- Social media (Instagram, Facebook, LinkedIn)
- Web search for "[restaurant name] parent company"
- Evidence of sister restaurants on company pages

For each restaurant group found, list ALL sister restaurants you can verify.
