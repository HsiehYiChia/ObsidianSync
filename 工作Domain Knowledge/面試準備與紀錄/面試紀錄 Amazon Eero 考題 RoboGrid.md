---

---

![[Untitled 37.png]]

![[Untitled 38.png]]

![[Untitled 39.png]]

```c++
#include <limits.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum 
{
  SUCCESS, /* If the command is successfully parsed */
  NULL_PARAM, /* If any pointer in the input is NULL */
  INVALID_UNDO_CMD /* If the requested undo command is not valid */
} FuncStatus_t;

typedef enum 
{
  FWD, /* +Y */
  BCK, /* -Y */
  RGT, /* +X */
  LFT, /* -X */
  UND /* Undo */
} CommandType_t;

typedef struct 
{
  CommandType_t cmd_type;
  int32_t value;
} Command_t;

FuncStatus_t ParseCommands(const Command_t cmds[], const uint32_t num_cmds, int32_t * out_x, int32_t * out_y)
{
    /* Warning: You can use stdout for debugging, but doing so will cause the test cases to fail. */
    if (cmds == nullptr || out_x == nullptr || out_y == nullptr)
        return NULL_PARAM;
        
    int32_t x = 0;
    int32_t y = 0;
    int32_t move_x = 0;
    int32_t move_y = 0;
    uint32_t last_cmd = UND;
    for (uint32_t i = 0; i < num_cmds; i++) {
        //printf("i=%d cmd_type=%d value=%d\n", i, cmds[i].cmd_type, cmds[i].value);
        switch(cmds[i].cmd_type) {
        case FWD:
            move_x = 0;
            move_y = 1 * cmds[i].value;
            break;
        case BCK:
            move_x = 0;
            move_y = -1 * cmds[i].value;
            break;
        case RGT:
            move_x = 1 * cmds[i].value;
            move_y = 0;
            break;
        case LFT:
            move_x = -1 * cmds[i].value;
            move_y = 0;
            break;
        case UND:
            if (last_cmd == UND || cmds[i].value != 1)
                return INVALID_UNDO_CMD;
            move_x *= -1;
            move_y *= -1;
        default:
            break;
        }
        x += move_x;
        y += move_y;
        last_cmd = cmds[i].cmd_type;
    }
    *out_x = x;
    *out_y = y;
    return SUCCESS;
}
```
