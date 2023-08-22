- 👋 Hi, I’m @Manul1919191
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Manul1919191/Manul1919191 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
¶tim mayچ

¶ٌ Given our assumption that p > q, the probability drops exponentially as the number of blocks the attacker has to catch up with increases. With the odds against him, if he doesn't make a lucky lunge forward early on, his chances become vanishingly small as he falls further behind.
We now consider how long the recipient of a new transaction needs to wait before being sufficiently certain the sender can't change the transaction. We assume the sender is an attacker who wants to make the recipient believe he paid him for a while, then switch it to pay back to himself after some time has passed. The receiver will be alerted when that happens, but the sender hopes it will be too late.
The receiver generates a new key pair and gives the public key to the sender shortly before signing. This prevents the sender from preparing a chain of blocks ahead of time by working on it continuously until he is lucky enough to get far enough ahead, then executing the transaction at that moment. Once the transaction is sent, the dishonest sender starts working in secret on a parallel chain containing an alternate version of his transaction.
The recipient waits until the transaction has been added to a block and z blocks have been linked after it. He doesn't know the exact amount of progress the attacker has made, but assuming the honest blocks took the average expected time per block, the attacker's potential progress will be a Poisson distribution with expected value:
 = z qp
To get the probability the attacker could still catch up now, we multiply the Poisson density for
each amount of progress he could have made by the probability he could catch up from that point:
∞ ke−{q/pz−k ifk≤z} ∑k=0 k!⋅ 1 ifkz
Rearranging to avoid summing the infinite tail of the distribution...
z ke− z−k 1−∑k=0 k! 1−q/p 
Converting to C code...
   #include <math.h>
   double AttackerSuccessProbability(double q, int z)
   {
       double p = 1.0 - q;
       double lambda = z * (q / p);
       double sum = 1.0;
       int i, k;
       for (k = 0; k <= z; k++)
       {
           double poisson = exp(-lambda);
           for (i = 1; i <= k; i++)
               poisson *= lambda / i;
           sum -= poisson * (1 - pow(q / p, z - k));
}
return sum; }ٌ
