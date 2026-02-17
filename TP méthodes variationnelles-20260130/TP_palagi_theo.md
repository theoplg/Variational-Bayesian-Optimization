# Réponses aux Questions

### Question 1 :

On cherche à résoudre : $E_1(u) = \|u - v\|^2 + \lambda \|\nabla u\|^2$.

Pour réaliser cela, `minimisation_quadratique` prépare des filtres finies $K_x = (1, -1)$ et $K_y = (1, -1)^T$. Ces filtres vont permettre d'approcher $\nabla u$, en x et en y, en discrétisant u.  
L'outil `resoud_quad` permet de réoudre le problème quadratique donné par `minimisation_quadratique` en passant par le domaine de fourier notamment avec la fft.

$$
\begin{aligned}
E_1(u) &= \|u - v\|^2 + \lambda \|\nabla u\|^2 \\
&= \|u - v\|^2 + \lambda \sum_{i,j} \left((K_x \ast u)^2(i,j) + (K_y \ast u)^2(i,j)\right)
\end{aligned}
$$

On a donc : $K_0 = \delta \quad K_1 = \sqrt{\lambda}K_x \quad K_2 = \sqrt{\lambda}K_y $   
On passe dasn le domaine de Fourier : 
$$\hat{E_1} = \sum_c \sum_w |\hat{K_c}(w)\hat{u}(w) - \hat{V_c}(x)|$$

## Question 2 :

- Lorsque $\lambda$ est très petit, l'image n'est presque pas débruitée. En effet, le terme de régularisation va devenir négligeable et l'algorithme va privilégier ce qui reste collé aux données. L'image u sera donc très proche de l'image bruitée v.
- Lorsque $\lambda$ est très grand par contre, l'image va avoir tendance à se lisser grandement. Le terme de régularisation va devenir plus important, ce qui va pénaliser les contours de l'image. L'image sera débruitée mais très floue.

## Question 3 :

On trouve que le $\lambda$ optimal pour que $ \|\tilde{u} - v\|^2 \sim \|u - v\|^2 $ est environ égale à $3, 3$.

## Question 4 :

Pour trouver le $\lambda$ optimal, l'algorithme est simple, on applique la méthode pour une grande plage de $\lambda$, on calcule l'erreur entre l'image parfaite et reconstruite que l'on stock dans une liste. On trouve ensuite l'indice dont l'argument est le minimum pour calculer le $\lambda$. On trouve $\lambda_{optimal} \approx 1, 27$.

En traçant $err = f(\lambda)$ on trouve que la courbe fait une parabole. Pour un $\lambda$ trop faible l'erreur augmente (car le bruit n'a pas été enlevé), pour un trop important l'erreur augmente aussi (car l'image reconstruite est floue).

## Descente de Gradient

On obtient pas le même résultat pour un pas de 1 et de $0, 1$. En effet, pour le pas de 1 l'énérgie augmente, ce qui signifie que la descente de gradient diverge. Si on prend des pas plus proches comme $0, 1$ ou $0, 5$ le résultat est le même seule la vitesse de convergence change.

## Projection de Chambolle

La méthode est beaucoup plus rapide et converge en mmoins d'itération que la descente de gradient. De plus, elle est plus précise, elle donne une erreur plus faible.

## Comparaison

En comparant les 2 méthodes ont trouve :

- Pour la descente de gradient : $\lambda{optimal} = 1, 2$
- Pour la projection de Chambolle : $\lambda{optimal} \approx 39, 8$

D'un point de vue qualitatif, la méthode de variation totale avec la projection de Chambolle est meilleure en tout point. Elle enlève mieux le bruit, garde les contours intacts. La descente de gradient elle floute les contours, et le bruit est encore un peu présent.
