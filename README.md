# Face Morphing
This project implements face morphing, a technique to generate intermediate images that blend two input faces seamlessly. 
The process involves detecting facial landmarks, triangulating them, and warping the images to create smooth transitions.

## Libraries Used
- OpenCV (cv2)
- NumPy
- Matplotlib
- Dlib
- Scipy.spatial.Delaunay

## Sample Output
Below is an example of the face morphing process and the final result:

![output_video](https://github.com/user-attachments/assets/cf2a9a96-1163-4ee9-8fd8-f011b28940e6)

## Implementation outline 
### Inputs 
Provide two face images for blending. For example:

https://unsplash.com/photos/woman-with-red-lipstick-and-black-mascara-vsWy6nchcOs
https://unsplash.com/photos/shallow-focus-photo-of-woman-standing-near-wall-eDVQwVMLMgU

<img width="550" alt="Screenshot 2024-11-16 at 12 44 12" src="https://github.com/user-attachments/assets/b1b5be15-18e2-4b8c-9f54-c0c91fdf8837">

### landmarks 
Use Dlib to detect facial landmarks for each input face. Optionally, add additional points manually (e.g., for the edges of the image) to improve the morphing effect.

<img width="538" alt="Screenshot 2024-11-16 at 12 55 24" src="https://github.com/user-attachments/assets/01f9263e-6014-4d06-8c8b-c01c6050cc2b">

### Meshing 
Generate a triangular mesh using Delaunay triangulation based on the facial landmarks. This mesh defines the regions for warping and blending.

<img width="577" alt="Screenshot 2024-11-16 at 12 49 11" src="https://github.com/user-attachments/assets/9cfab79c-db3d-40ba-b05f-7d8c42ee581d">

### Morphing
The morphing process combines two input images by ensuring a consistent triangular mesh structure, interpolating their landmarks, and blending pixel values to create a seamless transition.

1.Interpolate the landmarks from both input images to define the intermediate face: in_between = landmarks1 * (1 - w) + landmarks2 * w, 
where landmarks1 and landmarks2 are landmarks of the first and second images, and w is a weight parameter that determines the blending ratio.

2.Use the interpolated landmarks to create an intermediate triangular mesh. This mesh ensures correspondence between triangles in the intermediate face and the triangles in the two input images (img1, img2).

3.Compute an affine transformation matrix that maps the vertices of the triangle from the input images to the intermediate face.

4.For each pixel inside a triangle in the intermediate mesh, use the affine matrix to do inverse warp to locate its corresponding pixel in the input images. 

5.Blend the pixel colors from two imput images using w, pixel_value = (1−w)⋅warped_img1 + w⋅warped_img2, this ensures a smooth transition between the two images.

<img width="820" alt="Screenshot 2024-11-16 at 14 11 45" src="https://github.com/user-attachments/assets/55cf2204-68ea-4678-8aad-71be76678911">

### Further Development 
The current implementation uses a for loop to iterate over the blending weight w for generating intermediate images. This can be optimized by vectorizing w for simultaneous computation of multiple morphing steps, improving efficiency.
