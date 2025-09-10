---
layout: post
title: bai viet 1
categories: baiviet1 baiviet2
---

# Reinforcement Learning

# Khái niệm chung

### Định nghĩa: Là chiến lược học và thử sai của agent đối với môi trường mà agent tương tác

### Sự khác biệt giữa học cải thiện (reinforcement learning) với học có giám sát (supervised learning) và học không giám sát (unsupervised learning)

|  | Reinforcement Learning | Supervised Learning | Unsupervised Learning |
| --- | --- | --- | --- |
| Mục tiêu | Giúp agent thích nghi với môi trường mà nó tương tác | Giảm sự sai khác giữa giá trị dự đoán và giá trị thực tế | Học được pattern của dữ liệu |
| Cách tiến hành | Thử và sai, dựa trên cơ chế thưởng phạt đối với từng hành động của agent đối với môi trường | Dựa trên việc tối ưu hóa hàm mục tiêu (thường là hàm mất mát đo lường sai khác giá trị dự đoán và giá trị thực tế) | Phương pháp đo lường khoảng cách, phân phối dữ liệu |

# Markov Decision Process (MDP)

MDP gồm 6 thành phần chính, bao gồm:

- Agent
- Môi trường
- Trạng thái (state)
- Hành động (Action)
- Reward (Phần thưởng hoặc hình phạt)
- Policy: Cách agent đưa ra hành động (chiến lược) tại mỗi state

> Một trạng thái $S_t$ được xem là Markov khi và chỉ khi
> 

$$
P[S_{t+1}|S_{t}] = P[S_{t+1}|S_1,S_2,...S_t]
$$

Tính chất Markov: Trạng thái hiện tại của một agent phụ thuộc vào ngay trước đó của nó mà không phụ thuộc vào trạng thái từ ban đầu, xác suất $S_{t+1}$ chỉ phụ thuộc vào $S_t$.

Một Markov Process được định nghĩa bởi tập hợp $(S,P)$ trong đó $S$ là tập hợp tất cả trạng thái có thể có của agent trong môi trường (state space) (các trạng thái trong đó tuần theo Markov)

Công thức xác suất chuyển từ s sang s’:

$$
\mathcal{P}_{ss'}=P[S_{t+1}=s'|S_t=s]
$$

![image.png](image.png)

## Markov Reward Process (MRP)

MRP được định nghĩa bởi tập hợp $(S,P,R,\gamma)$, trong đó S là tập hợp các trạng thái, P là tập hợp các xác suất $P_{ss'}$, R là tập hợp các reward và $\gamma$ là discount factor

Công thức của reward trong MRP là:

$$
\mathcal{R}_s=E[R_{t+1}|S_t=s]
$$

$\mathcal{R}_s$ là reward kỳ vọng của tất cả các trạng thái mà $S_t$ có thể đạt tới từ s, phần thưởng sẽ được nhận khi agent ở trạng thái $S_t$.

## Công thức của Markov Decision Process

Như vậy một MDP được cấu thành bởi các yếu tố bao gồm $(S,A,P,R,\gamma)$, bổ sung A là tập hợp các hành động của agent

### Phần thưởng cuối (The return)

Khác với phần thưởng hiện tại, phần thưởng cuối được tính từ $S_t$ trở đi, phần thưởng cuối là mục tiêu tối ưu của thuật toán học tăng cường, có công thức như sau:

$$
	G_t=\sum_{k=0}^{\infty}{\gamma}^kR_{t+k+1}=R_{t+1}+\gamma R_{t+2} + ...
$$

Sự có mặt của discount factor $\gamma$ đảm bảo rằng các reward càng về sau thì có sự ảnh hưởng càng thấp, như vậy mô hình sẽ vừa muốn tối đa phần thưởng cuối nhưng tập trung vào các phương pháp để giúp cho việc đạt reward càng sớm càng tốt. $\gamma$ có giá trị nằm trong khoảng [0,1] đại diện cho sự bật định đối với các trạng thái và viễn cảnh đối vưới tương lai

Tuy nhiên, hầu như các trạng thái trong tương lai đều bất định, vì vậy ta cần biến đổi lại hàm tính phần thưởng cuối (sau đây gọi là State value function - SVF) tính kỳ vọng của các giá trị $G_t$ từ trạng thái $s$

$$
v(s)=E[G_t|s_t=s]
$$

### Policy

Được coi là chiến lược để agent đưa ra hành động dựa trên trạng thái, như vậy có thể coi policy thể hiện phân phối các hành động dựa trên state được cho trước, công thức cảu policy được biểu diễn như sau

$$
\pi(a|s)=P[A_t=a|S_t=s]
$$

Như vậy policy là thứ khiến cho mô hình có thể đưa ra hành động ngay sau khi nó được biết về thông tin trạng thái **hiện tại**

### Phương trình Bellman cho MRP

$$
\begin{aligned}
v(s) &= \mathbb{E}[G_t \mid S_t=s] \\
&= \mathbb{E}[R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots \mid S_t = s] \\
&= \mathbb{E}[R_{t+1}+\gamma(R_{t+2} + \gamma R_{t+3}+\cdots)|S_t=s] \\ 
&= \mathbb{E}[R_{t+1}+\gamma G_{t+1} | S_t=s] \\
&= \mathbb{E}[R_{t+1}+\gamma v(S_{t+1}) | S_t=s]
\end{aligned}
$$
