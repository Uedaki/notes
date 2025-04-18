# Bidirectional Path Tracing

## Introduction

One of the most common algorithm to solve the problem of global illumination in computer graphics, is using the Monte Carlo (MC) methods. It handles naturally most physical effects that happen in the real world, and can be apply to arbitrary surface (triangles, curves). It also has the nice effect to converge towards the correct solution. But this technique suffers from a major drawback which is noise. The most common MC technique is usually implemented in unidirectionnal pathtracer. The rays are generated from the camera and scatter through the scene to find a light source.

Bi-directional path tracing (BDPT) is a technique introduced in 1993, by Lafortune, and improved by Veach in 1994. The basic idea is to generate at the same time, paths from the camera and a set of selected light sources. All vertices on their respective paths are then connected using shadow rays and the appropriate contributions are added to the film. This technique was originally created to improve the convergence of indoor scene but it is also useful for rendering difficult light paths like caustics.

## Outline of the algorithm

The algorithms can be split in 2 distinct phases. The first phase consists on generating a pre-defined amount of samples from randomly selected light sources. We then scatter them through the scene multiple time and store each vertex of each paths into a structure. The second phase consists of integrating the camera rays. It is similar to the integration in a unidirectional pathtracer with the caveat that for each vertex of the path, we are trying to connect it to the vertices of the light vertices structure previously recorded.

![](./bdpt-path.svg)

```C++
Color iteration()
{
  Color L = 0.f;
  vector<LightVertex> lightVertices;
  for (size_t i = 0; i < NumLightPath; i++)
  {
    LightState lightState = generateLightSample();
    for (size_t j = 0; j < maxBounces; j++)
    {
      intersect();
      storeVertice(lightVertices);
      L += connectToCamera();
      scatter();
    }
  }

  for (size_t i = 0; i < samplesPerPixel; i++)
  {
    Ray ray = generateCameraSample();
    for (size_t j = 0; j < maxBounces; j++)
    {
      intersect();
      shade();
      L += doVertexConnections(lightVertices);
      scatter();
    }
  }
  return (L);
}
```

!!! Question
    'Bi-Directional Path Tracing' paper seems to suggest the Russian Roulette can also be applied on the light path. I need to double check!

!!! Tip
    It is possible to combine next event estimation with BDPT as shown in [smallVCM](https://github.com/SmallVCM/SmallVCM/blob/a13690f03d3414f76ba32b5f0e259e6e5d10ed7b/src/vertexcm.hxx#L491).
    ```C++
    color += cameraState.mThroughput *
                                DirectIllumination(cameraState, hitPoint, bsdf);
    ```

## Weights

## Photon mapping

## Reference
- [Bi-Directional Path Tracing](https://www.cs.princeton.edu/courses/archive/fall03/cs526/papers/lafortune93.pdf), E. Lafortune, Y. Willems, 1993
- [Bidirectional Estimators for Light Transport](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Veach94.pdf), Veach and Guibas 1994
- [Light Transport Simulation with Vertex Connection and Merging](https://cgg.mff.cuni.cz/~jaroslav/papers/2012-vcm/2012-vcm-paper.pdf), Georgiev er al 2012
- [Small VCM renderer](http://www.smallvcm.com/), Georgiev er al
- [Simple Open-source Ray Tracing (SORT)](https://sort-renderer.com/), Jiayin Cao

## Future reference
- [Naive Bidirectional Path Tracing](https://agraphicsguynotes.com/posts/naive_bidirectional_path_tracing/), Jiayin Cao 2016
- [Practical implementation of MIS in Bidirectional Path Tracing](https://agraphicsguynotes.com/posts/practical_implementation_of_mis_in_bidirectional_path_tracing/), Jiayin Cao 2016
