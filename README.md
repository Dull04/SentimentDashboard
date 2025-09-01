## How To Run
- clone this Repo
- npm install
- npm run dev (Mininum Node Js version 20.19+ or 22.12+)

## Features

### 1. Dropdown Filter
- Allows users to select a specific channel or view all channels at once.
- All charts, tables, and summaries dynamically update based on the selected filter.

### 2. Channel Summary
- Provides a quick overview of total sentiment counts.
- Displayed in summary cards:
  - Positive
  - Neutral
  - Negative

### 3. Sentiment Table List
- Detailed sentiment data per channel, including:
  - Number of Positive, Neutral, and Negative mentions.
  - Total mentions per channel.
  - Percentage of each sentiment (% Positive, % Neutral, % Negative).
- Includes **search functionality** to filter specific channels.
- Supports **sortable columns** for flexible data viewing.

### 4. Sentiment Distribution Chart (Donut Chart)
- Visual representation of overall sentiment distribution.
- Shows percentage breakdown of Positive, Neutral, and Negative sentiments.

### 5. Daily Trend Chart (Bar Chart)
- Displays sentiment trends over time.
- Useful for analyzing how sentiment changes on a daily basis.

### 6. Channel Comparison Chart (Bar Chart)
- Compares sentiment across different platforms (News, Instagram, YouTube, Facebook, TikTok, Twitter).
- Highlights which channels are more positive or negative.

## 📌 Additional Notes

### 📦 Libraries Used
- [Vue 3](https://vuejs.org/) → Main frontend framework.
- [Vite](https://vitejs.dev/) → Build tool & development server.
- [Bootstrap 5](https://getbootstrap.com/) → CSS framework for styling and layout.
- [Bootstrap Icons](https://icons.getbootstrap.com/) → Official Bootstrap icon set.
- [Chart.js](https://www.chartjs.org/) → Canvas-based charting library.
- [Chart.js Datalabels](https://chartjs-plugin-datalabels.netlify.app/) → Plugin to add labels on Chart.js charts.
- [Vue Chart.js](https://vue-chartjs.org/) → Official Vue wrapper for Chart.js.
- [ApexCharts](https://apexcharts.com/) → JavaScript charting library.
- [Vue3 ApexCharts](https://apexcharts.com/docs/vue-charts/) → Vue 3 wrapper for ApexCharts.

### 📝 Assumptions
- The project is running with **Node.js v18+**.
- Sentiment data is loaded locally from a JSON file (`package.json` file), not fetched from an external API.
- A modern browser (latest Chrome/Edge/Firefox) is required for full compatibility.
