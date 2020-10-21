---
layout: post
title:  "Stay skeptical"
date:   2020-10-18
categories: update
---
I have been working on a project related to physics informed neural network in recent months. The content is about solving partial differential equations through neural networks. The difference from traditional neural networks is that we don’t need to use any label data, just use equations to construct Loss function.

while constructing the loss function, I need to use a complex equation, the coefficients of the equation need to be solved by using KL expansion. Big brother Tang has done research on KL development before. I directly adopted the theoretical analysis of the Tang.

![KL](/source/OneDimKL.png)

According to the formula provided by the big brother, I quickly implement it using python, but the coefficients obtained were deviated from expectations. Since there have been mistakes caused by incorrect understanding of the formula before, I suspect that there is a problem with my understanding. After communicating with the big brother several times, I firmly believe that there is no problem with my understanding. Then I suspected that there was a problem with the code details. However, after dozens of audits, I still couldn't solve the problem. Asking for help from the mentor, still unable to solve the problem. After tossing for more than a week, I communicated with the big brother again, and finally found that the formula provided by the big brother was wrong.

Why I have never suspected that the formula is wrong? Perhaps it is because the code of the big brother has been written into the project in the group, and it has been used and tested by many team members. However, the formula provided by Tang comes from his comments, and his code does not directly use comments. Anyway, problem is solved, and the loss converged.
![loss](/source/pinn.png)