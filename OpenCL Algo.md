
### Math base
The algorithm is based on original [[KAZE#Nonlinear Diffusion Space]]. After some rewriting we do have follows:

$$
\begin{array}{left}
	L_{0} \ \ \ {-\ is\ the\ original\ image} \\
	\widetilde{L} = L_{0} \circledast \stackrel{5 \times 5}{K}_{\sigma=1} \\
	G = g_{23}(k, |\nabla  \widetilde{L}|^2) \\
	D = \frac{1}{2} G \circledast \begin{bsmallmatrix}
		0 && 1 && 0 \\
		1 && 4 && 1 \\
		0 && 1 && 0 \\
	\end{bsmallmatrix} \\
	\left [ \\
		\begin{array} {ll}
			L = \frac{1}{2} G \circledast \begin{bsmallmatrix}0&&0&&0\\1&&1&&0\\0&&0&&0\end{bsmallmatrix} / D\\
			R = \frac{1}{2} G \circledast \begin{bsmallmatrix}0&&0&&0\\0&&1&&1\\0&&0&&0\end{bsmallmatrix} / D\\
			U = \frac{1}{2} G \circledast \begin{bsmallmatrix}0&&1&&0\\0&&1&&0\\0&&0&&0\end{bsmallmatrix} / D\\
			D = \frac{1}{2} G \circledast \begin{bsmallmatrix}0&&0&&0\\0&&1&&0\\0&&1&&0\end{bsmallmatrix} / D\\
			g = L_{0} / D\\
		\end{array}
	\right.
\end{array}
$$
