---

---
```c++
Please use this Google doc during your interview (your interviewer will see what you write here). To free your hands for typing, we recommend using a headset or speakerphone.



bool SerialInputAvailable(int n);  // is >=1 word available?
int SerialInputRead(int n);  // reads 1 word from input n

bool SerialOutputReady(int n);  // is there space in output queue?
void SerialOutputWrite(int n, int payload);  // copies word into output queue
void thread_create(void *handler, int dir, int port);


#3 N input M output X Thread

vector<int> dst(n);
vector<queue<int>> vq_out(m);

Enum {
	In,
	out
};
void run(int dir, int port) {
If (dir == in) {
	If (SerialInputAvailable(port)) {
		If (dst[port] == -1) {
			Dst[port] = SerialInputRead(port);
		} else {
			Int payload = SerialInputRead(port);
			Vq_out[dst[port]].push(payload);
			Dst[port] = -1;
		}
	}
}

If (dir == out) {
	if(SerialOutputReady(port) && !vq_out[port].empty()) {
		SerialOutputWrite(port, vq_out[port].front());
		vq_out[port].pop();
	}
}
}
void main() {
for (int i = 0; i < n; i++)
thread_create(run, in, i);
for (int i = 0; i < m; i++)
	thread_create(run, out, i);

}

#2 N input M output 1 Thread

vector<int> dst(n);
vector<queue<int>> vq_out(m);

void run() {
for (int i = 0; vq_in.size(); i++) {
if (SerialInputAvailable(i)) {
		if (dst[i] == -1) {
			dst[i] = SerialInputRead(i);
} else {
	int payload = SerialInputRead(i);
	vq_out[dst[i]].push(payload);
	dst[i] = -1;
}
	}
}

	for (int i = 0; i < vq_out.size(); i++) {
		if (!vq_out[i].empty() && SerialOutputReady(i)) {
			SerialOutputWrite(i, vq_out[i].front());
			vq_out[i].pop();
}
}
}
#1 1 input N output 1 Thread
================================================

void run() {
	int dst = -1;
Int payload = 0;
vector<queue<int>> vq(n);
	if (SerialInputAvailable(0)) {
		if (dst == -1) {
			dst = SerialInputRead(0);
} else {
	payload = SerialInputRead(0);
	vq[dst].push(payload);
	dst = -1;
}
	}

	for (int i = 0; i < vq.size(); i++) {
		if (!vq[i].empty() && SerialOutputReady(i)) {
			SerialOutputWrite(i, vq[i].front());
			vq[i].pop();
}
}
}
```