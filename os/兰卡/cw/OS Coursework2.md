A number (N) of philosophers sit around a circular table which have only N forks on the table, one between each pair of philosophers
1. Think; 
2. Eat spaghetti
	- Philosophers may only pick up the forks to their immediate left or right
	- Eating requires two forks
Correctly model the behavior of the philosophers

1) PDF is expected.
2) Must attach the console result of running.


## Base

### clang
```c
/* Dining Philosopher Problem Using Semaphores */

#include <pthread.h> 
#include <semaphore.h> 
#include <stdio.h> 
#include <unistd.h>
#define N 5 
#define THINKING 2 
#define HUNGRY 1 
#define EATING 0 
#define LEFT (phnum + N - 1) % N 
#define RIGHT (phnum + 1) % N 
#define ID (phnum+1)
int state[N]; // state shows the Philosopher is eating, thinking or hungry
int phil[N] = { 0, 1, 2, 3, 4 };

sem_t mutex; //  Mutex is used when no two philosophers may access the pickup or putdown at the same time.(which will cause sth terrible)
sem_t S[N]; // The array is used to control the behavior of each philosopher. But, semaphores can result in deadlock due to programming errors.

/// @brief try to take the fork and eat
/// @param phnum index of the philosopher
void test(int phnum)
{
    if (state[phnum] == HUNGRY
        && state[LEFT] != EATING
        && state[RIGHT] != EATING) { // seems i can take the fork
        // state that eating 
        state[phnum] = EATING;
        sleep(1); // try eat

        printf("Philosopher %d takes fork %d and %d\n",
            ID, ID, LEFT + 1);
        printf("Philosopher %d is Eating\n", ID);

        sem_post(&S[phnum]);
    }
}

// take up chopsticks 
void take_fork(int phnum)
{
    sem_wait(&mutex); // wait

    // state that hungry 
    state[phnum] = HUNGRY;

    printf("Philosopher %d is Hungry\n", phnum + 1);

    // eat if neighbours are not eating 
    test(phnum); //try to take both forks around

    sem_post(&mutex); // send semaphore back

    // if unable to eat wait to be signalled 
    sem_wait(&S[phnum]); // waiting for the fork
    sleep(1);
}

// put down chopsticks 
void put_fork(int phnum)
{
    sem_wait(&mutex);

    // state that thinking 
    state[phnum] = THINKING;

    printf("Philosopher %d putting fork %d and %d down\n",
        ID, ID, LEFT + 1);
    printf("Philosopher %d is thinking\n", phnum + 1);

    test(LEFT);
    test(RIGHT);

    sem_post(&mutex);
}

void* philospher(void* num)
{
    while (1) {
        int* i = (int*)num; // like object in java, i can change the type void* to any pointer
        sleep(1);
        take_fork(*i);
        sleep(0); // sleep(0) means put me to the waitlist
        put_fork(*i);
    }
}

int main()
{
    int i;
    pthread_t thread_id[N];
    // initialize the semaphores 
    sem_init(&mutex, 0, 1);

    for (i = 0; i < N; i++)
        // init semaphores for every single Philosopher
        sem_init(&S[i], 0, 0);

    for (i = 0; i < N; i++) {
        // create philosopher processes 
        pthread_create(&thread_id[i], NULL,
            philospher, &phil[i]);
        printf("Philosopher %d is thinking\n", i + 1);
    }

    // enjoy eating!
    for (i = 0; i < N; i++)
        // int pthread_join(pthread_t thread, void **retval);
        pthread_join(thread_id[i], NULL);
}
```

Run:
```bash
gcc [filename] -lpthread
./a.out
```

Sample output:

```
Philosopher 1 is thinking
Philosopher 2 is thinking
Philosopher 3 is thinking
Philosopher 4 is thinking
Philosopher 5 is thinking
Philosopher 5 is Hungry
Philosopher 4 is Hungry
Philosopher 1 is Hungry
Philosopher 2 is Hungry
Philosopher 3 is Hungry
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 2 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 4 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 1 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 3 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 5 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 2 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 4 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 1 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 3 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 5 is Hungry
Philosopher 2 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 4 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 1 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 3 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 5 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 2 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 4 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 1 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 3 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 5 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 2 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 4 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 1 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 3 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 5 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
^C
ubuntu@VM-4-11-ubuntu:~/git$ ./a.out 
Philosopher 1 is thinking
Philosopher 2 is thinking
Philosopher 3 is thinking
Philosopher 4 is thinking
Philosopher 5 is thinking
Philosopher 1 is Hungry
Philosopher 4 is Hungry
Philosopher 2 is Hungry
Philosopher 5 is Hungry
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 3 is Hungry
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 5 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 1 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 4 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 2 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 5 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 1 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 4 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 2 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 5 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 1 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 4 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 2 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 5 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 1 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 4 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 2 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 5 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 1 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 4 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
Philosopher 1 takes fork 1 and 5
Philosopher 1 is Eating
Philosopher 2 is Hungry
Philosopher 3 putting fork 3 and 2 down
Philosopher 3 is thinking
Philosopher 4 takes fork 4 and 3
Philosopher 4 is Eating
Philosopher 5 is Hungry
Philosopher 1 putting fork 1 and 5 down
Philosopher 1 is thinking
Philosopher 2 takes fork 2 and 1
Philosopher 2 is Eating
Philosopher 3 is Hungry
Philosopher 4 putting fork 4 and 3 down
Philosopher 4 is thinking
Philosopher 5 takes fork 5 and 4
Philosopher 5 is Eating
Philosopher 1 is Hungry
Philosopher 2 putting fork 2 and 1 down
Philosopher 2 is thinking
Philosopher 3 takes fork 3 and 2
Philosopher 3 is Eating
Philosopher 4 is Hungry
Philosopher 5 putting fork 5 and 4 down
Philosopher 5 is thinking
```
