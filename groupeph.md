下面给你一套**可直接上手**的路线：用群论来判断电子–声子耦合（electron–phonon, e–ph）的**模式选择定则**。我把“为什么”“怎么做”“常见情形”“快速示例”“工具与资料”都整理在一起，你可以按部就班应用到任何晶体材料。

---

## 1) 目标与核心结论（一眼看懂版）

电子–声子一次耦合的矩阵元可写成（Bloch 规范）

$$
g_{mn\nu}(\mathbf k,\mathbf q)=\langle u_{m,\mathbf k+\mathbf q}\,|\, \Delta_{\mathbf q\nu}V \,|\, u_{n,\mathbf k}\rangle ,
$$

其中 $\Delta_{\mathbf q\nu}V$ 是$\mathbf q$支、$\nu$模的声子引起的周期位势微扰。**非零的群论条件是**（“不变子”准则）：

$$
\boxed{\ \Gamma_{m}(\mathbf k+\mathbf q)\ \otimes\ \Gamma_{\text{ph}}(\mathbf q,\nu)\ \otimes\ \Gamma_{n}(\mathbf k)\ \supset\ \Gamma_{1}\ }
$$

也就是**末态 × 声子 × 初态**的直积分解里必须含有**全对称表示** $\Gamma_1$（A$_1$、A$_{1g}$ 等）。同时满足动量守恒 $\mathbf k'=\mathbf k+\mathbf q+\mathbf G$（$\mathbf G$ 为倒格矢）。这一定则是从第一性原理 e–ph 理论和表示论直接推出的通用结果。([Physical Review Link Manager][1])

> 实操时用到的“群”不是整个点群/空间群，而是能把相关波矢留在自身“星”（star）里的**小群**。严格做法是对能同时保持 $\mathbf k,\mathbf k+\mathbf q,\mathbf q$ 这三者的**共同小群**做直积分解；在高对称点/线，这会显著约束能否耦合。([Bilbao Crystallographic Server][2])

---

## 2) 步骤化工作流（十步法）

1. **确定晶体空间群** $G$ 与布里渊区高对称点/线（$\Gamma, X, L, K,\dots$）。
   工具：Bilbao Crystallographic Server (BCS) 的 *POINT*, *KVEC*。([Bilbao Crystallographic Server][3])
2. **定电子态**：给出初态 $|n,\mathbf k\rangle$ 与末态 $|m,\mathbf k'\rangle$。
   识别它们在各自**小群** $G_{\mathbf k}$、$G_{\mathbf k'}$ 下的不可约表示（irrep）$\Gamma_n(\mathbf k),\,\Gamma_m(\mathbf k')$。
   工具：BCS 的 *REPRES* / *DSG/DPG*（含自旋–轨道时用双群）。([Bilbao Crystallographic Server][2])
3. **定声子**：选择 $\mathbf q=\mathbf k'-\mathbf k+\mathbf G$ 及其分支 $\nu$，查声子在小群 $G_{\mathbf q}$ 下的 irrep $\Gamma_{\text{ph}}(\mathbf q,\nu)$。
   工具：BCS 的 *MECHANICAL REP.*, *PHONON* 模块。([Bilbao Crystallographic Server][3])
4. **找共同小群**：$G_{\text{com}}=G_{\mathbf k}\cap G_{\mathbf k'}\cap G_{\mathbf q}$。不少“同谷”（$\mathbf k'=\mathbf k+\mathbf q$ 且三者在同一路径附近）简化为用 $G_{\mathbf k_0}$（$\mathbf k_0$ 为高对称点/线）。([Bilbao Crystallographic Server][3])
5. **做直积分解**：把 $\Gamma_{m}\otimes\Gamma_{\text{ph}}\otimes\Gamma_{n}$ 在 $G_{\text{com}}$ 下分解，若含 $\Gamma_1$ 则**允许**，否则**禁戒**。
   教科书与表格：Dresselhaus 等；BCS/Cracknell–Love 记号一致。([UW Faculty][4])
6. **考虑简并**：若有 E、T 等简并表示，矩阵元会分解成少数几个**不变参数**（类似 Wigner–Eckart 定理），具体角向依赖由 Clebsch–Gordan 系数给出。([UW Faculty][4])
7. **自旋与双群**：有 SOC 时，必须用双点群/双空间群（$\Gamma_6,\Gamma_7,\Gamma_8$…）。禁戒/允许可能改变。([Bilbao Crystallographic Server][3])
8. **宇称/反演**（若有 $i$）：$\Gamma_g$ 与 $\Gamma_u$ 的乘积遵从 $g\times g=g,\ g\times u=u,\ u\times u=g$。因此在**严格 $\mathbf q=\mathbf 0$** 的一阶过程里，\*\*同宇称（g↔g 或 u↔u）\*\*必须配 **g 型**声子，\*\*异宇称（g↔u）\*\*必须配 **u 型**声子。实际散射多在小但非零 $\mathbf q$ 发生，此时共同小群常降到 $C_1$，禁戒会放宽。([UW Faculty][4])
9. **多声子/高阶过程**：两声子过程把 $\Gamma_{\text{ph}}$ 换成 $\Gamma_{\text{ph1}}\otimes\Gamma_{\text{ph2}}$ 再判。([UW Faculty][4])
10. **数值校验**：第一性原理 DFPT/EPW 计算的 $g_{mn\nu}(\mathbf k,\mathbf q)$ 应与群论“零/非零”结论一致（差别多因数值误差或忽略 SOC、微小对称性破缺）。([Physical Review Link Manager][1])

---

## 3) 三个高频场景与要点

### A. 同谷（小 $\mathbf q$）带内散射

* 常发生在高对称点附近；若带是**非简并**（如 A$_1$），只与**同表示**或与该表示直积能出 A$_1$ 的声子耦合。
* 在\*\*$C_{3v}$\*\* 的 $\Gamma$ 点：电子若属 **E**，则

  * $E\otimes A_1\otimes E = A_1\oplus A_2\oplus E\Rightarrow$ 允许；
  * $E\otimes A_2\otimes E = A_1\oplus A_2\oplus E\Rightarrow$ 允许；
  * $E\otimes E\otimes E$ 也含 $A_1\Rightarrow$ 允许。
    这说明“简并带 × 任意 $\Gamma$ 点声子”往往允许。表与乘法规则可在标准群论资料中查到。([UW Faculty][4])

### B. 反演对称晶体的宇称规则（$\mathbf q=\mathbf 0$）

* 见上“第 8 步”。例如选择定则会区分 Raman（g）与 IR（u）类声子与电子态的耦合模式。注意：真实输运散射来自小但非零 $\mathbf q$，宇称禁戒可能被解除。([UW Faculty][4])

### C. 间谷（大 $\mathbf q$）散射与 Kohn 异常

* 在石墨/石墨烯，**$\Gamma$-E$_{2g}$** 与 **K-A$_1'$** 模与 $\pi$ 电子强耦合（著名的两个 Kohn 异常），这既是 Raman **G**、**D/2D** 峰物理的核心，也体现了“只有满足群论直积含 $A_1$ 的声子才强耦”的选择性。([Physical Review Link Manager][5])

---

## 4) 迷你演练：以石墨烯为例（思路模板）

1. **点群/小群**：石墨烯整体点群 $D_{6h}$；K 点小群等价于 $D_{3h}$。$\pi$ 带在 K 的电子态属二维表示（常记为 $E''$ 一类，具体记号依赖所用表）。
2. **强耦合声子**：

* $\Gamma$ 点的 $E_{2g}$（G 峰）能与同谷电子强耦；
* K 点的 $A_1'$ 能作**间谷**散射（D/2D 峰）。

3. **判据**：对 K 点间谷过程，取共同小群（能把两个谷与对应 $\mathbf q\approx \mathbf K$ 保持在自身星内）；做
   $\Gamma_{\pi}(\mathbf K')\otimes \Gamma_{A_1'}(\mathbf K)\otimes \Gamma_{\pi}(\mathbf K)$ 的分解，含 $A_1$ ⇒ 允许，且耦合强（Kohn 异常的群论根源）。以上均与实验与一性原理相符。([Physical Review Link Manager][5])

---

## 5) 实操清单（你可以照着做）

* **查表**：到 BCS 的 *KVEC* 找高对称 $\mathbf k,\mathbf q$，用 *REPRES* / *MECHANICAL REP.* 取电子/声子不可约表示标签；必要时用 *DPG/DSG*（含 SOC）。([Bilbao Crystallographic Server][3])
* **直积**：用 Dresselhaus 的直积表/角色表，做 $\Gamma_m\otimes\Gamma_{\text{ph}}\otimes\Gamma_n$ 分解，看是否含 $\Gamma_1$。([UW Faculty][4])
* **验证**：用 DFPT（Quantum ESPRESSO/ABINIT/VASP）得 $\omega_{\mathbf q\nu}$ 与 $g_{mn\nu}$，或用 EPW/Wannier 化进行网格插值；看“群论零”的矩阵元是否数值上趋近 0。理论表达式与计算流程见 Giustino 综述。([Physical Review Link Manager][1])

---

## 6) 常见坑

* **忘了 SOC 用双群** → 选择定则可能错。解决：用双点群/双空间群 irrep。([Bilbao Crystallographic Server][3])
* **把严格 $\mathbf q=0$ 的宇称规则直接套到小 $\mathbf q$** → 实际散射在小 $\mathbf q$ 下常被“解禁”。([UW Faculty][4])
* **忽视“共同小群”** → 尤其在间谷过程，必须用能同时照顾 $\mathbf k,\mathbf k',\mathbf q$ 的对称性。([Bilbao Crystallographic Server][3])

---

## 7) 进一步阅读 / 速查资料

* **第一性原理 e–ph 理论综述**：Giustino, *Rev. Mod. Phys.* 89, 015003 (2017) 与勘误（2019）。内容涵盖矩阵元定义、动量守恒、材料实例。([Physical Review Link Manager][1])
* **群论与选择定则教材**：Dresselhaus《Group Theory: Application to the Physics of Solids》（含直积、投影算符、选择定则章）。([UW Faculty][4])
* **在线工具**：Bilbao Crystallographic Server（REPRES/KVEC/MECHANICAL REP./DPG/DSG/Compatibility Relations）。([Bilbao Crystallographic Server][3])
* **典型实例（石墨/石墨烯）**：$\Gamma$-E$_{2g}$ 与 K-A$_1'$ 的 Kohn 异常与强 e–ph 耦合（解释 G、D/2D 峰的选择性）。([Physical Review Link Manager][5])
* **材料特例（纤锌矿 GaN 等）**：群论分析可系统给出谷内/谷间散射的允许/禁戒规则。([arXiv][6])

---

## 8) 你可以马上试的两个小练习

1. **$C_{3v}$ 的 $\Gamma$ 点：** 设电子初末态都为 **E**，声子为 **A$_1$/A$_2$/E**，逐一做
   $E\otimes \Gamma_{\text{ph}}\otimes E$ 分解，判断是否含 $A_1$。
2. **含反演的 $D_{2h}$ 晶体：** 设 $\Gamma$ 点导带（g）↔ 价带（u）的跃迁，问**一阶 $\mathbf q=0$** 下哪些声子（g / u）能耦合？用宇称乘法一眼判定（答案：仅 u 型）。([UW Faculty][4])

---

如果你告诉我**具体材料/空间群**、**电子初末态的 $\mathbf k$** 与**考虑的声子 $\mathbf q$**，我可以**直接替你跑完上述 10 步**，把直积分解与是否允许的结论列成清单。

[1]: https://link.aps.org/doi/10.1103/RevModPhys.89.015003?utm_source=chatgpt.com "Electron-phonon interactions from first principles"
[2]: https://www.cryst.ehu.es/rep/repres.html?utm_source=chatgpt.com "REPRES - Bilbao Crystallographic Server - EHU"
[3]: https://www.cryst.ehu.es/?utm_source=chatgpt.com "Bilbao Crystallographic Server"
[4]: https://faculty.washington.edu/srsharpe/507/dresselhaus.pdf?utm_source=chatgpt.com "Applications of Group Theory to the Physics of Solids"
[5]: https://link.aps.org/doi/10.1103/PhysRevLett.93.185503?utm_source=chatgpt.com "Kohn Anomalies and Electron-Phonon Interactions in Graphite"
[6]: https://arxiv.org/pdf/1310.0079?utm_source=chatgpt.com "arXiv:1310.0079v1 [cond-mat.mtrl-sci] 30 Sep 2013"
