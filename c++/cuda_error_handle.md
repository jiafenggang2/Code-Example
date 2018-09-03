
# In CUDA
**1. Define Error Type**

```
enum cudaError{
    cudaSuccess = 0,
    cudaErrorMissingConfiguration = 1,
    ...,
}
```
**2. Define Map From cudaError to String**
```
extern const char* CUDARTAPI cudaGetErrorString(cudaError_t error);
```
**3. Define Exception Handle Error**
```
class cuda_error: public std::runtime_error
{
public:
    cuda_error(cudaError code):m_errorCode(code){}
    const char* what() const NOEXCEPT{
      std::ostringstream os;
      os<<"FILE: "<<__FILE__<<,LINE: "<<__LINE__<<",ERROE CODE: "<<m_errorCode<<
        <<":<<cudaGetErrorString(m_errorCode)<<std::endl;
    }
private:
    cudaError_t m_errorCode;
};
```
**4. Define Error Check Function**
```
#define CheckCudaError(cudaFuncRet){  \
  if(cudaFuncRet != cudaSuccess){ \
    throw cuda_error(cudaFuncRet);  \
  } \
}
```
**5. Use**
```
CheckCudaError(cudaMalloc(...));
```
