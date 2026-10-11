# Diagnosing Revenue Growth Deceleration in a Multi-Market SaaS Business

This is a root cause analysis of why revenue growth in a 6 month span for tripla--a b2b company that sells booking technology. Revenue slowed from **36.7% to 8.2% year on year** in Q4 2025, presented in 2026. Analysis was done using Tableau, SQL and Google Sheets. 
All data is synthetic and does not present actual figures.

## Presentation

[![Watch the presentation](https://img.youtube.com/vi/tWLmkKSi3EI/maxresdefault.jpg)](https://www.youtube.com/watch?v=tWLmkKSi3EI)

*5-minute walkthrough of the analysis and recommendations. Click to watch on YouTube.*

📄 [Full 2-page report (PDF)](assets/RCA_analysis_2pager.pdf)

## Executive summary

- **What happened:** revenue growth fell from 36.7% YoY in Q3 to 8.2% in Q4 2025. Revenue dropped 21.9% quarter on quarter, compared with a normal Q4 dip of 1.3% in 2024.
- **Where:** Japan accounts for ¥55.8M of the ¥60.3M drop (93%).
- **What changed:** booking volume. Gross bookings in Japan fell 28%, while ADR and the cancellation rate stayed broadly flat.
- **Which hotels:** the 7 named hotels in the data grew 13%, while revenue from the rest of Japan fell 40%. The decline sits outside the hotels we can see.
- **Recommendation:** first check whether Japanese properties churned or paused (property-level bookings and contract status), then the booking funnel, then technical issues. Add a per-hotel early-warning alert so future drops are caught within weeks.

## Analytical approach

Each step narrows the problem, and each chart answers one question with an if-then test stated before looking at the data.

1. **What changed?** Compare revenue, bookings, ADR and cancellation rate year on year.
2. **Where?** Break the drop down by market.
3. **What weakened inside that market?** Gross bookings, net bookings, ADR and cancellations for Japan.
4. **Through which channels?** Check whether one channel drove it.
5. **Which hotels?** Compare the 7 named hotels with the rest of Japan.
6. **Driver tree:** combine the findings into one chain of causes.

Tools: Google Sheets (exploratory analysis), SQL (aggregation and YoY calculations), Tableau (drill-down charts), Miro (driver tree and investigation plan).

## Findings

### 1. Revenue fell in line with bookings, not price or cancellations

![How much did revenue growth slow in Q4?](assets/table.png)

Revenue growth fell 28.5 points, matched almost exactly by gross bookings (−27.0 pts) and net bookings (−28.8 pts). ADR held steady, and the cancellation rate rose only 2.2 points.

### 2. Japan drove the slowdown

![Which market drove the Q4 slowdown?](assets/Dashboard%201.png)

Japan fell 27% in Q4, against a normal Q4 dip of about 4% the year before, and accounts for 93% of the total drop.

### 3. Inside Japan, fewer bookings came in

![What weakened inside Japan?](assets/Dashboard%202.png)

Gross bookings fell 28% and net bookings fell by a similar amount. ADR and the cancellation rate stayed flat.

### 4. The decline is outside the 7 named hotels

![Is Japan's decline spread across its hotels?](assets/Dashboard%203.png)

The 7 named hotels grew 13% (¥53.4M → ¥60.5M), while the rest of Japan fell 40% (¥156.3M → ¥93.5M).

### Driver tree

![Driver tree](assets/driver-tree.gif)

## Recommendations

![Next steps and recommendations](assets/next-steps-and-recommendations.gif)

A prioritised investigation, each with the data needed, diagnostic KPIs and recovery KPIs:

1. **Property or account issue (first).** Pull monthly bookings per Japanese hotel and check contract status. If hotels dropped to zero and their contracts ended, that's churn, so focus on retention and win-back.
2. **Conversion funnel issue.** If contracts are fine, analyse the booking funnel (traffic, search, booking starts, checkout) and test fixes at the biggest drop-off.
3. **Technical issue.** If the funnel is stable, review uptime, error rates and integration logs for the affected hotels.

**North star metric:** Japan gross bookings YoY, returning to its pre-Q4 growth rate.
**Quick win:** a per-hotel alert when monthly bookings fall well below that hotel's normal level for the season.

**Also watch Indonesia:** gross bookings keep growing, but the cancellation rate doubled to about 20%, so net bookings stalled. It didn't cause the Q4 slowdown, but it's getting worse.

![Are cancellations eating into Indonesia's growth?](assets/Dashboard%204.png)

## Dataset

**Origin.** Synthetic data provided as part of a BI case study for a hotel direct-booking technology company. It covers January 2024 – December 2025 across three markets (Japan, Indonesia, the Philippines), in Japanese yen. All figures are synthetic and do not reflect real company performance.

| Table | Grain | Coverage |
|---|---|---|
| Bookings by Market | market × month | Jan 2024 – Dec 2025 |
| Revenue by Channel | channel × month | Jan 2024 – Dec 2025 |
| Hotels | hotel × quarter | 2024-Q4 – 2025-Q4 (7 selected Japanese hotels) |

"Net" means after cancellations. ADR = net revenue ÷ net room-nights.

## Limitations

- **Aggregated data.** No booking-level records, property counts or contract status, so the analysis can locate the decline but not confirm its cause.
- **Partial hotel coverage.** Only 7 Japanese hotels (about a quarter of Japan's revenue in Q3 2025). "Rest of Japan" is calculated as the market total minus these hotels, so it depends on both tables using the same definitions.
- **Short hotel history.** Hotel data starts in 2024-Q4, giving one year-on-year comparison.
- **Estimated hotel ADR.** The hotel table assumes 1.9 room-nights per booking, so hotel-level ADR was not used.
- **Channels aren't split by market**, so channel trends can't be isolated to Japan.

## Repository contents

```
├── README.md
└── assets/
    ├── RCA_analysis_2pager.pdf
    ├── table.png              first finding
    ├── Dashboard 1.png        market
    ├── Dashboard 2.png        inside Japan
    ├── Dashboard 3.png        hotels
    ├── Dashboard 4.png        Indonesia
    ├── driver-tree.gif
    └── next-steps-and-recommendations.gif
```
