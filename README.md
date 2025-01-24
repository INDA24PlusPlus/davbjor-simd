# davbjor-simd
A benchmarking test for davbjor-raytracer to benchmark the usage of SIMD instructions vs single threaded cpu instructions.

## Result
Through benchmarking with C++ standard library <crono>, testing the time for the pathtracer to run, the program failed to produce a faster version using SIMD than using compiler optimized version of standard C++.

```
Without optimization    |  SIMD optimization

iteration 0/15
time: 1186 ms
iteration 1/15
time: 1199 ms
iteration 2/15
time: 1196 ms
iteration 3/15
time: 1197 ms
iteration 4/15
time: 1207 ms
iteration 5/15
time: 1196 ms
iteration 6/15
time: 1212 ms
iteration 7/15
time: 1210 ms
iteration 8/15
time: 1202 ms
iteration 9/15
time: 1197 ms
iteration 10/15
time: 1224 ms
iteration 11/15
time: 1223 ms
iteration 12/15
time: 1255 ms
iteration 13/15
time: 1224 ms
iteration 14/15
time: 1199 ms
Execution: 18132 ms
vs
iteration 0/15
time: 1696 ms
iteration 1/15
time: 1702 ms
iteration 2/15
time: 1769 ms
iteration 3/15
time: 1770 ms
iteration 4/15
time: 1924 ms
iteration 5/15
time: 1895 ms
iteration 6/15
time: 2040 ms
iteration 7/15
time: 1847 ms
iteration 8/15
time: 1741 ms
iteration 9/15
time: 1830 ms
iteration 10/15
time: 1959 ms
iteration 11/15
time: 1797 ms
iteration 12/15
time: 1763 ms
iteration 13/15
time: 1746 ms
iteration 14/15
time: 1765 ms
Execution time: 27251 ms
```

## Example of SIMD code:
An example of the code changed, in this case vector dot-product:
```
double dot(const vec3 &u, const vec3 &v){
    double r = 0;
    r += u.e[0] * v.e[0];
    r += u.e[1] * v.e[1];
    r += u.e[2] * v.e[2];
    return r;
}
```
Is replaced for a SIMD function:
```
float simd_dot(vec4 a, vec4 b){
    const __m128 xmm_a = _mm_load_ps(&a.x);
    const __m128 xmm_b = _mm_load_ps(&b.x);
    const __m128 xmm_res = _mm_dp_ps(xmm_a, xmm_b, 0xff);
    return _mm_cvtss_f32(xmm_res);
}
```
