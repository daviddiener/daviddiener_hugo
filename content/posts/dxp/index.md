---
title: Exploring Context-Sensitive Augmented Reality - A Seminar Project Overview
date: 2022-08-30
tags:
  - studies
cover:
    image: "cover.png"
---

## Introduction

In the ever-evolving landscape of Extended Reality (XR) technology, Augmented Reality (AR) holds a special place. This seminar project aims to contribute to the existing body of research by focusing on the practical implementation of context-sensitive AR applications. The project explores two key aspects of context sensitivity: Spatial Arrangement and Physical Factors.

<object data="DXP_Poster_DavidDiener.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="DXP_Poster_DavidDiener.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="DXP_Poster_DavidDiener.pdf">Download Poster</a>.</p>
    </embed>
</object>

## The Importance of Context-Sensitivity

Context-sensitivity in AR is not just a buzzword; it's a necessity for creating immersive and interactive experiences. My paper focuses on two pivotal aspects of context-sensitivity: "Spatial Arrangement" and "Physical Factors"[^1].

### Spatial Arrangement

Spatial Arrangement is all about how external information is organized and displayed within the AR application. In my research, I developed a surface detection system that adapts the interface based on the user's environment. This system builds upon the foundational work of Grubert et al. (2017), who described the importance of adapting the interface based on context[^1].

The surface detection system uses the Mixed Reality Toolkit's Scene Understanding API to classify surfaces as 'Walkable' or 'Obstacle.' This classification allows for more interactive and realistic AR experiences, as objects can be placed or interacted with based on these surface types[^2].

### Physical Factors

Physical Factors refer to the environmental elements that are not under the control of the AR system or the user. These could include lighting conditions, physical obstacles, and more. My paper presents a real-time 3D Navigation system that takes into account these physical factors[^2].

For the 3D Navigation system, I employed Mercuna, a third-party library, to navigate the spatial geometry. This system uses a data structure called 'Nav Octree' to represent the spatial geometry around the user. It also includes a 'Nav Seed' which is the starting point for navigation[^3].


## Limitations and Future Directions

- **Cold Start Problem**: The systems rely on the WSUO, which in turn depends on scanning the spatial environment. This means that surface detection and 3D Navigation are only possible after the environment has been successfully scanned.
- **Scanning Details**: The level of detail in spatial scanning is limited. For example, narrow geometric objects like table legs could not be reliably detected.

I see a lot of room for improvement and expansion. For instance, the surface detection system could be enhanced by considering additional spatial features like the curvature or texture of surfaces. Also, performance optimization is crucial, especially for mobile platforms like the HoloLens 2, where computational resources are limited.


## Conclusion

My seminar paper aims to push the boundaries of what's possible in context-sensitive AR. While there are challenges and limitations, the research sets a strong foundation for future work in this exciting and ever-evolving field.

{{< button text="Click here for a short demo" link="https://youtu.be/CQ1pEaBofvI?si=1dmjyDLJFS3hS-aJ" >}}

If you want to take a look at the full paper, you can download it below.

{{< button text="Download Paper" link="DXP_Implementation_of_a_context_sensitive_augmented_reality_application.pdf" >}}

## References

[^1]: Grubert, J., Langlotz, T., Zollmann, S., & Regenbrecht, H. (2017). Towards pervasive augmented reality: Context-awareness in augmented reality.

[^2]: Unity. (2022). Unity - scripting API: Collider.

[^3]: Mercuna. (2022). Mercuna - 3d navigation for games.