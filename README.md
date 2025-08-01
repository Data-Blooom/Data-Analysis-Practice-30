# Advanced Sentiment Analysis Dashboard with Power BI

## 🚀 Project Overview

This project presents a comprehensive sentiment analysis dashboard built with Microsoft Power BI. It dissects a dataset of **417,000 text messages** to uncover deep insights into emotional patterns, frequencies, and the intriguing relationship between sentiment type and message length.

This dashboard is not just a data presentation; it's a powerful tool for understanding human expression through text, demonstrating advanced data modeling, DAX proficiency, and sophisticated data visualization techniques.

---

## ✨ Key Insights & Strategic Findings

> This analysis reveals that while "Joy" is the most frequently expressed emotion, the overall emotional landscape is dominated by negative sentiments.

* **Emotional Distribution:** "Joy" constitutes the largest single emotional segment (**33.8%**), followed closely by "Sadness" (**29.1%**). However, when aggregated, **Negative** emotions (Sadness, Anger, Fear) outnumber **Positive** emotions (Joy, Love, Surprise) **226K to 191K**.
* **The "Length vs. Frequency" Anomaly:** The scatter plot provides the most compelling insight. Emotions with lower frequency, such as **"Love" (عاشقانه)**, correlate with a higher average message length. Conversely, high-frequency emotions like **"Joy" (شادی)** are typically expressed in shorter messages. This suggests that expressing deeper, more nuanced emotions may require more textual context.
* **Rarest Emotion as a Signal:** "Surprise" is the least common emotion (**3.6%**), making its appearance a potentially strong signal in contexts like customer feedback analysis.

---

## 🛠️ Tools & Technologies

* **Microsoft Power BI Desktop:** Core tool for data modeling, DAX, and visualization.
* **DAX (Data Analysis Expressions):** Used extensively for creating calculated columns, measures, and summary tables.

---

## 🔬 Data Modeling & DAX Logic

The model was enhanced with several key DAX calculations to enable the analysis.

### Calculated Columns

These columns were added to the `Felling` table to enrich the dataset:

* **`اسم احساس` (Emotion Name):** Maps numeric sentiment codes to readable text strings.
    ```dax
    اسم احساس = SWITCH('Felling'[احساس], 0, "غمگین", 1, "شادی", 2, "عاشقانه", 3, "عصبانی", 4, "ترس", 5, "سوپرایز")
    ```
* **`نوع پیام` (Message Type):** Classifies each emotion as "مثبت" (Positive) or "منفی" (Negative).
    ```dax
    نوع پیام = SWITCH('Felling'[اسم احساس], "شادی", "مثبت", "سوپرایز", "مثبت", "عاشقانه", "مثبت", "عصبانی", "منفی", "ترس", "منفی", "غمگین", "منفی")
    ```
* **`طول پیام` (Message Length):** Calculates the character length of each message.
    ```dax
    طول پیام = LEN('Felling'[پیام])
    ```

### Calculated Table

A summary table was generated for aggregated analysis:
* **`جدول خلاصه سازی شده` (Summary Table):**
    ```dax
    جدول خلاصه سازی شده = SUMMARIZECOLUMNS('Felling'[اسم احساس], "تعداد احساسات", COUNTROWS('Felling'), "میانگین طول پیام", AVERAGE(Felling[طول پیام]))
    ```
    
### Key Measures

These measures power the main KPI cards and visuals on the dashboard:

```dax
// Counts the total number of messages (rows).
کل پیام ها = COUNTROWS('Felling')

// Calculates the average length of all messages.
میانگین طول پیام = AVERAGE(Felling[طول پیام])

// Dynamically finds the most frequent emotion.
غالب ترین احساس = CALCULATE(SELECTEDVALUE(Felling[اسم احساس]), TOPN(1, VALUES(Felling[اسم احساس]), CALCULATE(COUNTROWS('Felling')), DESC))

// Dynamically finds the least frequent emotion.
کمیاب ترین احساس = CALCULATE(SELECTEDVALUE(Felling[اسم احساس]), TOPN(1, VALUES(Felling[اسم احساس]), CALCULATE(COUNTROWS('Felling')), ASC))
```

### Design Note
* **Transparent Background:** The theme utilizes a transparent background setting (`"rgba(255, 255, 255, 0)"`) for visuals to create a modern, floating effect over the custom background.
