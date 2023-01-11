# CircleQueue

원형큐 사용을 보조해주는 라이브러리입니다. UART 메시지 처리를 위해 큐를 만들었습니다. 

## Code

circleQueue.h
```h
/*
 * circleQueue.h
 *
 *  Created on: Jan 11, 2023
 *      Author: SW2148
 */

#ifndef INC_CIRCLEQUEUE_H_
#define INC_CIRCLEQUEUE_H_

#define MAX_QUEUE_SIZE		(255)

typedef struct {
	uint8_t queue_buffer[MAX_QUEUE_SIZE];
	uint8_t head;
	uint8_t tail;
}circleQueue;

extern circleQueue CircleQueue;

void push(uint8_t new_data);
uint8_t pop(void);
uint8_t isEmpty(void);


#endif /* INC_CIRCLEQUEUE_H_ */
```

circleQueue.c
```C
/*
 * circleQueue.c
 *
 *  Created on: Jan 11, 2023
 *      Author: SW2148
 */

#include <stdint.h>
#include "circleQueue.h"

circleQueue CircleQueue;

//push: 새로운 데이터를 Queue buffer에 담는다.
void push(uint8_t new_data)
{
	//새로운 데이터를 queue에 넣는다.
	CircleQueue.queue_buffer[CircleQueue.head] = new_data;

	CircleQueue.head++;

	//head가 queue 마지막 위치에 도달했다면, 0으로 초기화
	if(CircleQueue.head >= MAX_QUEUE_SIZE){
		CircleQueue.head = 0;
	}

}

//pop: 가장 오래된 데이터를 가져온다.
uint8_t pop(void)
{
	//tail의 위치에 데이터를 가져온다.
	uint8_t pop_data = CircleQueue.queue_buffer[CircleQueue.tail];

	CircleQueue.tail++;

	if(CircleQueue.tail >= MAX_QUEUE_SIZE) {
		CircleQueue.tail = 0;
	}

	return pop_data;
}

uint8_t isEmpty(void)
{
	//head와 tail의 위치가 같으면 queue가 비어있음
	return CircleQueue.head == CircleQueue.tail;
}

```

