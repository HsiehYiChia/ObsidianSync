---

---

![[Untitled 40.png]]

![[Untitled 41.png]]

![[Untitled 42.png]]

![[Untitled 43.png]]

![[Untitled 44.png]]

```c++
#include <limits.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <inttypes.h>
#include <stdbool.h>

typedef enum 
{
  STATUS_SUCCESS,
  STATUS_BAD_POINTER,
  STATUS_BAD_ARRAY_LENGTH,
  STATUS_NO_SENSOR_DATA,
} FuncStatus_t;
FuncStatus_t TransformData(
    const size_t inputDataLength, const uint8_t inputDataValues[], const size_t outputDataLength, uint8_t outputDataValues[]) {
    /* Stub */
#if 0
    printf("inputDataLength=%d\n", inputDataLength);
    for (int i = 0; i < inputDataLength; i++)
        printf("0x%2x ", inputDataValues[i]);
    printf("\n");
    printf("outputDataLength=%d\n", outputDataLength);
#endif
    if (inputDataValues == nullptr)
        return STATUS_BAD_POINTER;
    else if (inputDataValues[0] == 0x0)
        return STATUS_NO_SENSOR_DATA;
    else if (inputDataLength < 9 || outputDataLength < 33)
        return STATUS_BAD_ARRAY_LENGTH;
    
    uint8_t valid_cnt = 0;
    for (uint8_t i = 0; i < 8; i++) {
        bool valid = (inputDataValues[0] >> i) & 0x01;
        if (!valid)
            continue;
            
        uint32_t power = (i == 0) ? 1 : (2 << (i-1));
        uint32_t sensor_value = inputDataValues[i+1] * power;
        outputDataValues[1 + 4*i + 0] = sensor_value & 0xff;
        outputDataValues[1 + 4*i + 1] = sensor_value >> 8 & 0xff;
        outputDataValues[1 + 4*i + 2] = sensor_value >> 16 & 0xff;
        outputDataValues[1 + 4*i + 3] = sensor_value >> 24 & 0xff;
        valid_cnt++;
    }
    outputDataValues[0] = valid_cnt;
    return STATUS_SUCCESS;
}
```