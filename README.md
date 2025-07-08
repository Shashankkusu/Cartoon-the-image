# Cartooning the Image 

This repository contains a Jupyter Notebook (`Cartooning-the-image.ipynb`) demonstrating how to convert an input image into a cartoon-like version using OpenCV in Python. The code is intended to run in Google Colab, but can be adapted for local use with minor changes.

---

## How Does Cartoonization Work?

The cartoon effect is achieved by combining two main image processing techniques:

1. **Edge Detection**: The edges in the image are detected to create a sketch-like outline.
2. **Color Smoothing**: The colors in the image are smoothed to give a painting or cartoon-like flat appearance.

These steps are then combined using a bitwise operation to produce the final cartoon image.

---

## Step-by-step Explanation

### 1. Read the Image

```python
img = cv2.imread("/content/Input_image.png")
```
- Loads the input image using OpenCV.

### 2. Display the Original Image

```python
cv2_imshow(img)
```
- Shows the original image in the notebook.

### 3. Convert to Grayscale

```python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```
- Converts the image to grayscale, necessary for edge detection.

### 4. Apply Median Blur

```python
gray_blur = cv2.medianBlur(gray, 5)
```
- Removes noise and smooths the image while preserving edges.

### 5. Detect Edges with Adaptive Thresholding

```python
edges = cv2.adaptiveThreshold(
    gray_blur, 255,
    cv2.ADAPTIVE_THRESH_MEAN_C,
    cv2.THRESH_BINARY,
    blockSize=9, C=9
)
```
- Detects and emphasizes edges in the image, making a "sketch" effect.

### 6. Apply Bilateral Filter to Smooth Colors

```python
color = cv2.bilateralFilter(img, d=9, sigmaColor=250, sigmaSpace=250)
```
- Smooths out color regions while keeping edges sharp. This creates a painting-like effect.

### 7. Combine Edges with Color Image

```python
cartoon = cv2.bitwise_and(color, color, mask=edges)
```
- Combines the color-smoothed image with the edge mask to overlay the "sketch" on the simplified color image.

### 8. Display and Save the Results

```python
cv2_imshow(img)      # Original
cv2_imshow(edges)    # Edges
cv2_imshow(cartoon)  # Cartoon result
cv2.imwrite("cartoon_output.jpg", cartoon)
```
- Shows the different processing steps and saves the cartoonized image.

---

## How to Use

1. **Upload your image** to Colab (or place it in the appropriate directory).
2. **Update the image path** in the notebook if needed.
3. **Run the notebook cells** in order.
4. The cartoonized image will be displayed and saved as `cartoon_output.jpg`.

---

## Requirements

- Python 3
- OpenCV (`cv2`)
- Google Colab (or local Jupyter Notebook with display adjustments)

---

## Cartoonization Pipeline Diagram

```
Input Image
     |
     v
+-----------------+
| Grayscale       |
| Conversion      |
+-----------------+
     |
     v
+-----------------+
| Median Blur     |
+-----------------+
     |
     v
+-------------------------+
| Adaptive Threshold      |-------> Edge Mask (Sketch)
+-------------------------+
     |
     |
     v
+-------------------------+
| Bilateral Filter        |-------> Smoothed Color Image
+-------------------------+

  Edge Mask + Smoothed Color
          |
          v
  +----------------------+
  | Bitwise AND Combine  |
  +----------------------+
          |
          v
    Cartoon Image Output
```

---

## Example Output

The notebook will display the original image, the edge mask, and the cartoonized result. The cartoonized image has bold edges and flat, posterized colors, mimicking a hand-drawn cartoon or painting.

---

## License

Open-source code for educational purposes.
