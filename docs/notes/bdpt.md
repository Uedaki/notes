# Bidirectional Path Tracing

## Theory
Why are we trying to do this

![](./bdpt-path.svg)

## Implementation skeleton

```C++
void iteration()
{
  for (size_t i = 0; i < NumLightPath; i++)
  {
    LightState lightState = GenerateLightSample();
    for (size_t j = 0; j < maxBounces; j++)
    {
      intersect();
      storeVertice();
      connectToCamera();
      scatter();
    }
  }

  for (size_t i = 0; i < samplesPerPixel; i++)
  {
    Ray ray = GenerateCameraSample();
    for (size_t j = 0; j < maxBounces; j++)
    {
      intersect();
      shade();
      doVertexConnections();
      doVertexMerging();
      scatter();
    }
  }
}
```

!!! Tip
    It is possible to combine next event estimation with BDPT as shown in [smallVCM](https://github.com/SmallVCM/SmallVCM/blob/a13690f03d3414f76ba32b5f0e259e6e5d10ed7b/src/vertexcm.hxx#L491).
    ```C++
    color += cameraState.mThroughput *
                                DirectIllumination(cameraState, hitPoint, bsdf);
    ```

## Estimators

### What is an estimator?

### Unidirectional;

## Reference
- [Bidirectional Estimators for Light Transport](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Veach94.pdf), Veach and Guibas 1994
- [Light Transport Simulation with Vertex Connection and Merging](https://cgg.mff.cuni.cz/~jaroslav/papers/2012-vcm/2012-vcm-paper.pdf), Georgiev er al 2012
- [Small VCM renderer](http://www.smallvcm.com/), Georgiev er al
- [Simple Open-source Ray Tracing (SORT)](https://sort-renderer.com/), Jiayin Cao

## Future reference
- [Naive Bidirectional Path Tracing](https://agraphicsguynotes.com/posts/naive_bidirectional_path_tracing/), Jiayin Cao 2016
- [Practical implementation of MIS in Bidirectional Path Tracing](https://agraphicsguynotes.com/posts/practical_implementation_of_mis_in_bidirectional_path_tracing/), Jiayin Cao 2016