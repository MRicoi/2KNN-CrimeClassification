# Investigating K-NN's Limits with SF Crime Data

This project was a practical experiment to test the effectiveness of the K-Nearest Neighbors (K-NN) algorithm on a complex, real-world classification problem: predicting crime categories in San Francisco.

### 1. My Hypothesis: A Test of Proximity

I wanted to test a simple idea: is **proximity** (both geographic and temporal) a strong enough signal to predict the type of a crime?

K-NN is the perfect algorithm for this test, as its logic is entirely based on finding the closest "neighbors" to a data point to classify it. My hypothesis was that crimes occurring near each other, at similar times, would likely be of the same category.

### 2. My Method: The Math Behind the Code

To test this hypothesis fairly, the process was as follows:

* **Feature Engineering:** First, I extracted useful information from the timestamp column, creating numerical features like `Hour`, `DayOfWeek` and `Month`.
* **Standardization (`StandardScaler`):** To ensure the distance calculation was fair, all features were standardized. The math is straightforward: `new_value = (original_value - mean) / standard_deviation`. This puts all features on the same scale, preventing any single feature from dominating the others.
* **The K-NN Logic:** With all data on the same scale, K-NN calculated the "closeness" between a new crime and all crimes in the training history using **Euclidean Distance**. Based on these distances, it found the 'K' nearest neighbors and performed a **majority vote** among them to decide the new crime's category.

### 3. My Findings: The Visual Proof

The best way to evaluate the outcome was by visualizing the Confusion Matrix. The heatmap below tells the story of the experiment:

*(Here you would embed the image of your heatmap)*
![Confusion Matrix Heatmap](2KNN-Crime/heatmap_crime.png)

The analysis of this chart was clear:
-   The model only learned to accurately predict the most common crime: `LARCENY/THEFT`. This is obvious from the single bright yellow spot on the diagonal.
-   It was extremely confused by other categories. The strong vertical line in the `LARCENY/THEFT` column shows that when in doubt, the model's default bet was almost always "larceny/theft".
-   Rarer crimes were almost completely ignored, as the model could not find dense "neighborhoods" of these examples to learn from.

### 4. My Conclusion: What I Learned About K-NN

My initial hypothesis was proven wrong. Simple **proximity is not a sufficient predictor** to solve such a complex problem with 39 overlapping classes.

K-NN failed here because the "neighborhood" of a crime in San Francisco is too "noisy." Many different types of crime can occur in the same location. The algorithm would need well-defined crime "districts" to work well, but the reality of the data is a complex mix.

This project was successful in **demonstrating the limitations of K-NN**. The conclusion is that for this problem, an algorithm that can learn more **complex rules and patterns** beyond just distance.
