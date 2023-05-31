# tjctf 2023: ezdlp

### We're trying to solve $8999^x \equiv s \pmod{p}$, for particularly large $s$ and $p$. The general ideas is to prime factorize $p$ as $p_1 \cdot p_2 \cdot p_3 \cdot \cdots \cdot p_k$, then solve each $8999^{x_i} \equiv (s\pmod{p_i}) \pmod {p_i}$ for each $i$ using a DLP solver algorithm like little step-big step. Once we find each $x_i$, we can find $x$ using CRT, since we know $x \equiv x_i \pmod{p_i-1}$.

### Alternatively, we could just use https://www.alpertron.com.ar/DILOG.HTM and get our flag, 
**tj{2610447885456977094876326862907909435102076425842 57043466661851716310947137425165260749103252026125 75130356252792856014835908436517926646322189289728 46201179414851392693034338208138871407788931829734 96657400614827431379486354760882647512121209069484 50722431680198753238856720828205708702161666784517}**


