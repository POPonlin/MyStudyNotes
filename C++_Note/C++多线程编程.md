# 基础入门
## 线程库的基本使用
```cpp
#include<iostream>
#include<thread>
#include<string>
using namespace std;

void TestFunction(const string& message )
{
	cout << message << endl;
}

int main()
{
	//创建线程，传递函数参数
	thread t1(TestFunction, "HelloWorld");
	//1. join（），主线程等待其它线程执行完后才结束，会阻塞在join位置
	//t1.join();
	//2. detach(), 将此线程在后台运行，主线程结束后仍会继续运行。不会报错
	//t1.detach();
	//3. joinable(), 判断线程是否可以进行detch或join，返回一个布尔值
	if (t1.joinable())
	{
		t1.join();
	}
	cout << "over";

	return 0;
}


```
## 线程函数中的未定义错误
```cpp
#include<iostream>
#include<thread>
#include<memory>



class A
{
public:
	void foo(int& a)
	{
		std::cout << a;
	}
private:
	friend void MyFunction(A* obj);
	void foo2()
	{
		std::cout << "www";
	}
};

void MyFunction(A* obj)
{
	obj->foo2();
}

int main()
{
	using namespace std;
	//这样写会导致未定义错误，因为1是临时变量
	//thread t1(foo, 1);
	//解决方法
	//int x = 1;
	//thread t1(foo, ref(x));

	//int* ptr = new int(1);
	//thread t1(foo, ptr);
	//如果该线程后续还要进行，指针就被提前删除的话,会报错
	//delete ptr;
	//解决方法：使用智能指针
	//shared_ptr<A> ptr = make_shared<A>();
	//thread t1(&A::foo, ptr);

	//访问私有成员函数
	A a;
	thread t1(MyFunction, &a);

	if (t1.joinable())
	{
		t1.join();
	}

	return 0;
}
```
## 互斥量解决多线程数据共享问题
在多个线程中共享数据时，需要注意线程安全问题。如果多个线程同时访问同一个变量，并且其中至少有一个线程对该变量进行了写操作，那么就会出现数据竞争问题。数据竞争可能会导致程序崩溃、产生未定义的结果，或者得到错误的结果。
为了避免数据竞争问题，需要使用同步机制来确保多个线程之间对共享数据的访问是安全的。常见的同步机制包括互斥量、条件变量、原子操作等。
**互斥锁，mutex, 需要头文件#include<mutex>**
mutex mtx 

- mtx.lock() 上锁，只允许一个线程访问，解锁之前，其它线程均不能访问
- mtx.unlock()解锁
```cpp
#include<iostream>
#include<thread>
#include<string>
#include <mutex>
using namespace std;

int a = 0;
mutex mtx;
void func()
{
	for (int i = 0; i < 10000; i++)
	{
        //如果去掉mtx.lock() 和 mtx.unlock()
        //就会导致两个线程可能同时访问一个数据，导致结果偏差
		mtx.lock();
		a += 1;
		mtx.unlock();
	}
}

int main()
{
	thread t1(func);
	thread t2(func);

	t1.join();
	t2.join();

	cout << a << endl;
	return 0;
}

```
## 互斥量死锁
现有两个线程T1和T2，他们需要对两个互斥量mtx1和mtx2进行访问
按照以下顺序访问，会导致死锁问题：

- T1先获取mtx1的所以权，再获取mtx2的所有权
- T2先获取mtx2的所有权，再获取mtx1的所有权

如果两线程同时进行，会导致双方互相等待对方释放互斥锁，导致无限等待，导致死锁
```cpp
#include<iostream>
#include<thread>
#include<string>
#include <mutex>
using namespace std;

mutex m1, m2;
void func1()
{
	for (int i = 0; i < 1000; i++)
	{
		m1.lock();
		m2.lock();
		m1.unlock();
		m2.unlock();
	}
}
void func2()
{
	for (int i = 0; i < 1000; i++)
	{
		m2.lock();
		m1.lock();
		m1.unlock();
		m2.unlock();
	}
}

int main()
{
	thread t1(func1);
	thread t2(func2);

	t1.join();
	t2.join();

	cout << "over" << endl;
	return 0;
}

```
## lock_guard和unique_lock
std::lock_guard 是c++标准库中的一种互斥量封装类，用于保护共享数据，防止多个线程同时访问同一资源而导致数据竞争问题

- 当构造函数被调用，该互斥量会自动上锁
- 当析构函数被调用，该互斥量会自动解锁
- lock_guard对象不能被复制或移动，所以只能在局部作用域中使用
```cpp
template <class _Mutex>	//类模板
class _NODISCARD_LOCK lock_guard { // class with destructor that unlocks a mutex
public:
    using mutex_type = _Mutex;

    explicit lock_guard(_Mutex& _Mtx) : _MyMutex(_Mtx) { // 在构造函数中上锁，explicit指明不能使用隐式转换
        _MyMutex.lock();
    }

    lock_guard(_Mutex& _Mtx, adopt_lock_t) : _MyMutex(_Mtx) {} // 构造重载，不上锁

    ~lock_guard() noexcept {
        _MyMutex.unlock();
    }

    lock_guard(const lock_guard&)            = delete;	//删除拷贝构造
    lock_guard& operator=(const lock_guard&) = delete;	//删除赋值

private:
    _Mutex& _MyMutex;
};
```
```cpp
#include<iostream>
#include<thread>
#include<string>
#include <mutex>
using namespace std;

int a = 0;
mutex mtx;
void func()
{
	for (int i = 0; i < 10000; i++)
	{
		lock_guard<mutex> lg(mtx);
        //lock_guard<mutex> lg(mtx, std::adopt_lock);
		a += 1;

	}
}

int main()
{
	thread t1(func);
	thread t2(func);

	t1.join();
	t2.join();

	cout << a << endl;
	return 0;
}
```
std::unique_lock是c++标准库中的一种互斥量封装类，用于在多线程程序中对互斥量进行加锁和解锁操作。可以对互斥量进行更灵活的管理，包括**延迟加锁、条件变量、超时**等。
```cpp
template <class _Mutex>
class unique_lock { // whizzy class with destructor that unlocks mutex
public:
    using mutex_type = _Mutex;

    unique_lock() noexcept : _Pmtx(nullptr), _Owns(false) {}

    _NODISCARD_CTOR_LOCK explicit unique_lock(_Mutex& _Mtx)
        : _Pmtx(_STD addressof(_Mtx)), _Owns(false) { // construct and lock
        _Pmtx->lock();
        _Owns = true;
    }

    _NODISCARD_CTOR_LOCK unique_lock(_Mutex& _Mtx, adopt_lock_t)
        : _Pmtx(_STD addressof(_Mtx)), _Owns(true) {} // construct and assume already locked

    unique_lock(_Mutex& _Mtx, defer_lock_t) noexcept
        : _Pmtx(_STD addressof(_Mtx)), _Owns(false) {} // construct but don't lock

    _NODISCARD_CTOR_LOCK unique_lock(_Mutex& _Mtx, try_to_lock_t)
        : _Pmtx(_STD addressof(_Mtx)), _Owns(_Pmtx->try_lock()) {} // construct and try to lock

    template <class _Rep, class _Period>
    _NODISCARD_CTOR_LOCK unique_lock(_Mutex& _Mtx, const chrono::duration<_Rep, _Period>& _Rel_time)
        : _Pmtx(_STD addressof(_Mtx)), _Owns(_Pmtx->try_lock_for(_Rel_time)) {} // construct and lock with timeout

    template <class _Clock, class _Duration>
    _NODISCARD_CTOR_LOCK unique_lock(_Mutex& _Mtx, const chrono::time_point<_Clock, _Duration>& _Abs_time)
        : _Pmtx(_STD addressof(_Mtx)), _Owns(_Pmtx->try_lock_until(_Abs_time)) {
        // construct and lock with timeout
#if _HAS_CXX20
        static_assert(chrono::is_clock_v<_Clock>, "Clock type required");
#endif // _HAS_CXX20
    }

    _NODISCARD_CTOR_LOCK unique_lock(_Mutex& _Mtx, const xtime* _Abs_time)
        : _Pmtx(_STD addressof(_Mtx)), _Owns(false) { // try to lock until _Abs_time
        _Owns = _Pmtx->try_lock_until(_Abs_time);
    }

    _NODISCARD_CTOR_LOCK unique_lock(unique_lock&& _Other) noexcept : _Pmtx(_Other._Pmtx), _Owns(_Other._Owns) {
        _Other._Pmtx = nullptr;
        _Other._Owns = false;
    }

    unique_lock& operator=(unique_lock&& _Other) {
        if (this != _STD addressof(_Other)) {
            if (_Owns) {
                _Pmtx->unlock();
            }

            _Pmtx        = _Other._Pmtx;
            _Owns        = _Other._Owns;
            _Other._Pmtx = nullptr;
            _Other._Owns = false;
        }
        return *this;
    }

    ~unique_lock() noexcept {
        if (_Owns) {
            _Pmtx->unlock();
        }
    }

    unique_lock(const unique_lock&)            = delete;
    unique_lock& operator=(const unique_lock&) = delete;

    void lock() { // lock the mutex
        _Validate();
        _Pmtx->lock();
        _Owns = true;
    }

    _NODISCARD_TRY_CHANGE_STATE bool try_lock() {
        _Validate();
        _Owns = _Pmtx->try_lock();
        return _Owns;
    }

    template <class _Rep, class _Period>
    _NODISCARD_TRY_CHANGE_STATE bool try_lock_for(const chrono::duration<_Rep, _Period>& _Rel_time) {
        _Validate();
        _Owns = _Pmtx->try_lock_for(_Rel_time);
        return _Owns;
    }

    template <class _Clock, class _Duration>
    _NODISCARD_TRY_CHANGE_STATE bool try_lock_until(const chrono::time_point<_Clock, _Duration>& _Abs_time) {
#if _HAS_CXX20
        static_assert(chrono::is_clock_v<_Clock>, "Clock type required");
#endif // _HAS_CXX20
        _Validate();
        _Owns = _Pmtx->try_lock_until(_Abs_time);
        return _Owns;
    }

    _NODISCARD_TRY_CHANGE_STATE bool try_lock_until(const xtime* _Abs_time) {
        _Validate();
        _Owns = _Pmtx->try_lock_until(_Abs_time);
        return _Owns;
    }

    void unlock() {
        if (!_Pmtx || !_Owns) {
            _Throw_system_error(errc::operation_not_permitted);
        }

        _Pmtx->unlock();
        _Owns = false;
    }

    void swap(unique_lock& _Other) noexcept {
        _STD swap(_Pmtx, _Other._Pmtx);
        _STD swap(_Owns, _Other._Owns);
    }

    _Mutex* release() noexcept {
        _Mutex* _Res = _Pmtx;
        _Pmtx        = nullptr;
        _Owns        = false;
        return _Res;
    }

    _NODISCARD bool owns_lock() const noexcept {
        return _Owns;
    }

    explicit operator bool() const noexcept {
        return _Owns;
    }

    _NODISCARD _Mutex* mutex() const noexcept {
        return _Pmtx;
    }

private:
    _Mutex* _Pmtx;
    bool _Owns;

    void _Validate() const { // check if the mutex can be locked
        if (!_Pmtx) {
            _Throw_system_error(errc::operation_not_permitted);
        }

        if (_Owns) {
            _Throw_system_error(errc::resource_deadlock_would_occur);
        }
    }
};

```
```cpp
void func()
{
	for (int i = 0; i < 10000; i++)
	{
		unique_lock<mutex> ul(mtx);
		a += 1;
	}
}
```
```cpp
int a = 0;
timed_mutex mtx;
void func()
{
	for (int i = 0; i < 2; i++)
	{
        //先设定unique_lock构造不上锁
		unique_lock<timed_mutex> ul(mtx, defer_lock);
		if (ul.try_lock_for(chrono::seconds(2)))
		{
			this_thread::sleep_for(chrono::seconds(1));
			a++;
		}		
	}
}
```

- lock() 尝试对互斥量进行加锁操作，如果当前互斥量已经被其它线程持有，则当前线程会被阻塞，直到互斥量被成功加速
- try_lock() 尝试对互斥量量进行加锁操作，如果当前互斥量已经被其它线程持有，则函数立即返回false，否则返回true
- try_lock_for(const chrono::duration<_Rep, _Period>& _Rel_time) 尝试对互斥量量进行加锁操作，如果当前互斥量已经被其它线程持有，则当前线程会被阻塞，直到互斥量被成功加锁，或者超过了指定的**时间**
- try_lock_until(const chrono::time_point<_Clock, _Duration>& _Abs_time) 尝试对互斥量量进行加锁操作，如果当前互斥量已经被其它线程持有，则当前先线程会被阻塞，直到互斥量被成功加锁，或者超过了指定**时间点**
- unlock() 对互斥量进行解锁操作

构造函数：

- unique_lock() noexcept 默认构造函数，创建一个未关联任何互斥量的unique_lock对象
- explicit unique_lock(_Mutex& _Mtx) 使用给定互斥量进行初始化，并对互斥量上锁，不允许隐式转换
- unique_lock(_Mutex& _Mtx, defer_lock_t) noexcept 使用给定互斥量进行初始化，但不进行上锁操作
- unique_lock(_Mutex& _Mtx, try_to_lock_t) 使用给定互斥量进行初始化，并对互斥量进行上锁，上锁失败，则创建的unique_lock对象不与任何互斥量关联
- unique_lock(_Mutex& _Mtx, adopt_lock_t) 使用给定的互斥量进行初始化，并假设该互斥量已经被当前线程重新加锁
## call_once与使用场景
单例模式，用于确保某个类只能创建一个实例。全局唯一，在多线程环境下，需要考虑线程安全的问题

- 懒汉模式：在第一次被引用的时候，才会将自己实例化
- 饿汉模式：在自己被加载时就将自己实例化
```cpp
class Log
{
public:
	static Log& GetInstance()
	{
		static Log* log = NULL;
		if (!log) log = new Log;

		return *log;
	}

	void PrintLog(string msg)
	{
		cout << __TIME__ << ' ' << msg << std::endl;
	}
private:
	Log() {};
	Log(const Log& log) = delete;
	Log& operator=(const Log& log) = delete;
};

int main()
{
	Log::GetInstance().PrintLog("ERROR");
}
```
```cpp
class Log
{
public:
	//静态成员，编译期已经创建好，在全局区
	static Log& GetInstance()
	{
		static Log log;
		return log;
	}

	void PrintLog(string msg)
	{
		cout << __TIME__ << ' ' << msg << std::endl;
	}
private:
	Log() {};
	Log(const Log& log) = delete;
	Log& operator=(const Log& log) = delete;
};

int main()
{
	Log::GetInstance().PrintLog("ERROR");
}
```
```cpp
class Singleton
{
public:
	static Singleton& GetInstance()
	{
		std::call_once(m_onceFlag, &Singleton::Init);//确保函数只会被调用一次
		return *m_instance;
	}
	void SetData(int data)
	{
		m_data = data;
	}
	int GetData() const
	{
		return m_data;
	}

private:
	static void Init()
	{
		m_instance.reset(new Singleton);
	}

	Singleton() {};	//私有构造
	Singleton(const Singleton&) = delete;	//删除拷贝构造
	Singleton& operator=(const Singleton&) = delete;	//删除=重载
	static std::unique_ptr<Singleton> m_instance;	//智能指针，自动管理内存，并保证只有一个对象
	static std::once_flag m_onceFlag;	//用于标记函数是否已经被调用
	int m_data = 0;
};
```
```cpp
template<class Callable, class... Args>
void call_once(std::once_flag& flag, Callable&& func, Args&&... args);
```
其中，`flag` 是一个 `std::once_flag` 类型的对象，用于标记函数是否已经被调用；`func` 是需要被调用的函数或可调用对象；`args` 是函数或可调用对象的参数。
 `std::call_once` 函数会抛出 `std::system_error` 异常，如果在调用 `func` 函数时发生了异常，则该异常会被传递给调用者。
## condition_variable与使用场景

1. 创建一个condition_variable 对象
2. 创建一个mutex 互斥锁对象，用来保护共享资源（临界资源）
3. 在需要等待条件变量的地方
   1. 使用unique_lock<mutex> 对象锁定互斥锁
   2. 调用condition_variable::wait()、condition_variable::wait_for()、condition_variable::wait_unitl() 函数等待条件变量
4. 在其他线程中通知需要等待的线程时，调用condition_variable::notify_one()或condition_variable::notify_all() 函数通知等待进程
```cpp
#include<iostream>
#include<string>
#include<condition_variable>
#include<queue>

using namespace std;

queue<int> m_queue;
mutex mtx;
condition_variable cv;

void Producer()
{
	for (int i = 0; i < 10; i++)
	{
		unique_lock<mutex> lock(mtx);
		m_queue.push(i);
        //通知的时候对方会再次尝试获取锁
		cv.notify_one();
		cout << "Producer: " << i << endl;
	}
	this_thread::sleep_for(chrono::microseconds(100));
}

void Consumer()
{
	while (1)
	{
		unique_lock<mutex> lock(mtx);
        //此处wait操作会释放锁，不会造成死锁问题
		cv.wait(lock, []() {
			return !m_queue.empty();
			});
		int value = m_queue.front();
		m_queue.pop();
		cout << "Consumer: " << value << endl;
	}
}

int main()
{
	thread t1(Producer);
	thread t2(Consumer);

	t1.join();
	t2.join();
	return 0;
}
```
使用 `std::condition_variable` 可以实现线程的等待和通知机制，从而在多线程环境中实现同步操作。在生产者-消费者模型中，使用 `std::condition_variable` 可以让消费者线程等待生产者线程生产数据后再进行消费，避免了数据丢失或者数据不一致的问题。
## C++11跨平台线程池
线程的创建和销毁很消耗资源和时间 
 
```cpp
#include<iostream>
#include<queue>
#include<mutex>
#include<condition_variable>
#include<thread>
#include<functional>

class ThreadPool
{
public:
	ThreadPool(const int& num) : is_Stop(false)
	{
		for (int i = 0; i < num; i++)
		{
			threads.emplace_back([this] {
				while (1)
				{
					std::unique_lock<std::mutex> lock(mtx);
					condition.wait(lock, [this] { return is_Stop || !tasks.empty(); });
					if (is_Stop && tasks.empty())
					{
						return;
					}
					std::function<void()> task(std::move(tasks.front()));
					tasks.pop();
					lock.unlock();
					task();
				}
			});
		}
	}

	~ThreadPool()
	{
		{
			std::unique_lock<std::mutex> lock(mtx);
			is_Stop = true;
		}
		condition.notify_all();
		for (auto& t : threads)
		{
			t.join();
		}
	}

	template<typename F, typename... Args>
	void enqueue(F&& f, Args&&... args)
	{
		std::function<void()> task = std::bind(std::forward<F>(f), std::forward<Args>(args)...);
		{
			std::unique_lock<std::mutex> lock(mtx);
			tasks.emplace(std::move(task));
		}
		condition.notify_one();
	}

private:
	std::queue<std::function<void()>> tasks;
	std::vector<std::thread> threads;
	std::mutex mtx;
	std::condition_variable condition;
	bool is_Stop;
};

int main()
{
	ThreadPool pool(4);
	for (int i = 0; i < 10; i++)
	{
		pool.enqueue([i] {
			std::cout << "task: " << i << " is running" << std::endl;
			std::this_thread::sleep_for(std::chrono::seconds(1));
			std::cout << "task: " << i << " is done" << std::endl;
		});
	}
	return 0;
}
```
