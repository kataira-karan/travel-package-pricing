# ✈️ Summer 2026 Travel Package Pricing: Power BI Dashboard

**FP20 Analytics x ZoomCharts Data Challenge #40**

An interactive Power BI report that compares **10,666 summer 2026 holiday packages** from 5 European origins (London, Manchester, Dublin, Berlin, Amsterdam) to **21 destinations in Spain, Portugal, France and Italy**. It brings price, flights, weather, tourism crowding and destination quality together in one view, so travellers and travel businesses can answer: *where should we go, when, and what will it really cost?*

![Dashboard preview](assets/dashboard-preview.png)
<!-- Export a screenshot of each page to assets/ to show it here -->
<img width="1167" height="668" alt="image" src="https://github.com/user-attachments/assets/05a8ec9e-a4b4-43c0-b050-ec3a25513fee" />
<img width="1169" height="647" alt="image" src="https://github.com/user-attachments/assets/509d5663-1706-47fc-a14e-a13bd62e67b7" />

---

## 📂 Repository contents

| Path | Description |
|---|---|
| `dashboard/PackagePricingDashboard.pbix` | The Power BI report (open with Power BI Desktop) |
| `Summer_2026_Holiday_Dataset_modified.xlsx` | Working dataset the report is built on |
| `Summer_2026_Holiday_Dataset.xlsx` | Original challenge dataset |
| `docs/data-model.md` | Tables, grains, relationships and modelling rules |
| `docs/dax-measures.md` | Every DAX measure used in the report |
| `Travel Dataset Model Example Power BI.png` | Reference data model diagram |

---

## 🛠️ Major steps taken

### 1. Understanding the data
- Profiled the dataset's 4 fact tables and 9 dimensions, and checked each table's **grain** before writing any measures:
  - one offer = origin + destination + departure + traveller type + accommodation + booking snapshot + trip length.
- Used the dataset's 42 built-in logic tests to confirm the data was internally consistent.

### 2. Building a star schema
- Loaded the data into a star schema: **Fact_Holiday_Offers, Fact_Weather, Fact_Tourism and Fact_Destination_Scores** around shared dimensions (Date, Destination, Region, Origin, Traveller Type, Accommodation Type, Holiday Category, Holiday Style).
- Set up one-to-many, single-direction relationships throughout.
- Used a **bridge table** for the many-to-many link between destinations and holiday styles.
- Kept tourism data at **region** level (not city), so cities in the same region aren't double counted.

### 3. Writing the DAX measures (28 in total, see [`docs/dax-measures.md`](docs/dax-measures.md))
- **Price:** average and median price per person, average price per night per person, cheapest offer, average return flight per person.
  - Costs are never summed across alternative offers. Each row is a *choice*, not a purchase, so measures use AVERAGE, MEDIAN and MIN.
- **Quality:** a **weighted destination score** built with `SUMX` × `SuggestedWeight` across 12 holiday categories (beaches, culture, food, nightlife and more).
- **Value Index:** weighted score ÷ median price per night, which highlights destinations that give the most quality per euro.
- **Crowding:** summer tourism pressure.
  - Uses `TREATAS` to push the selected destinations' regions onto the tourism table, because the relationship only filters from region to city.
  - Includes "Max Pressure City / Region" cards that use `REMOVEFILTERS` so they always show the overall peak.
- **Weather:** average comfort score, plus a dynamic "Warmest Destination" measure using `ADDCOLUMNS` + `TOPN`.

### 4. Designing the report

| Page | What it shows |
|---|---|
| 🏠 **Overview** | KPI cards (packages offered, median price per person, average price per night, direct flight %, weather comfort), price trend by departure month, cost per person by destination type, flight cost by traveller type, domestic vs international visitors by region |
| ✈️ **Destination** | City, country and type cards; a weather snapshot (temperature, sunshine, rainfall, humidity, wind); warmest and most crowded destination; a **score vs. cost bubble chart** (bubble = tourism pressure); a city × holiday-category score matrix |

The two pages are connected with page navigators and buttons, and there's a bookmark for resetting the view.

---

## 🔍 What the dashboard reveals

> ℹ️ The challenge data is **synthetic** (generated for the competition), so the findings show how the dashboard works rather than describing the real market.

**Pricing**
- The **median package costs about €923 per person**. The average is about **€211 per person per night**.
- **September is the cheapest month to depart** (about €190 per person per night), roughly **13% cheaper than July** (about €220), which is the peak.
- **Accommodation drives the price most.** Hostels average about €147 per person per night and five-star hotels about €448, 3× as much.
- **Booking early pays off.** Offers booked 90 days ahead average about €208 per person per night versus about €214 at 30 days. Event weeks carry the biggest uplift, at about €249.
- **The origin city matters.** London and Manchester travellers pay the least (about €192–196 per person per night) and Berlin travellers the most (about €244).

**Destinations**
- **Best value:** **Porto, Montpellier, Marseille and Valencia** have the highest Value Index, combining strong review scores with low nightly prices.
- **Most expensive:** **Funchal, Palma de Mallorca and Nice** (about €200–208 median per person per night).
- **Highest rated:** **Lisbon** (weighted score of about 83), **Barcelona** (about 80) and **Palermo** (about 80).
- **Warmest:** **Seville**, averaging about 32 °C, with highs up to 45 °C.
- **Most crowded:** **Palma de Mallorca / Balearic Islands** in August, the peak tourism pressure in the whole dataset.
- **81% of offers include a direct flight**, but **Florence has none**, and Funchal and Lisbon are below 45%.

**Travellers**
- **Families pay the least per person for flights** (about €154 versus about €175–180 for other traveller types), because child fares bring the per-person average down.
- Visitors split roughly **55% domestic / 45% international**. The islands and the Algarve attract the highest international share.

---

## 💼 How it can help a business

| Who | How they'd use it |
|---|---|
| **Tour operators and travel agencies** | Spot high-value, under-priced destinations (Porto, Valencia) to promote; build "shoulder season" September packages; bundle direct flights where they're scarce |
| **Pricing and revenue teams** | Track how price changes with lead time, season and events; benchmark each origin market; set early-booking discounts with evidence behind them |
| **Marketing teams** | Target campaigns by traveller type (families, couples, luxury); point crowd-averse travellers to quieter, comparably rated alternatives to Palma or Venice |
| **Destination and tourism boards** | Monitor peak-month pressure and the domestic vs international mix to plan capacity and spread demand across the season |
| **Travellers** | Pick a destination that balances budget, weather, crowds and quality, and choose when to book |

---

## 🧰 Tools and techniques
**Power BI Desktop** · **DAX** (CALCULATE, TREATAS, REMOVEFILTERS, SUMX, TOPN, RANKX) · **Power Query** · Star schema modelling · Bookmarks and page navigation

## 🚀 How to open
1. Clone or download this repo.
2. Open `dashboard/PackagePricingDashboard.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
3. If prompted, point the data source to `Summer_2026_Holiday_Dataset_modified.xlsx` in this folder.

## 🙏 Acknowledgements
Dataset and challenge by **FP20 Analytics** in partnership with **ZoomCharts**.

## 👤 Author
**Karan Kataria**: feedback and connections welcome on LinkedIn!
