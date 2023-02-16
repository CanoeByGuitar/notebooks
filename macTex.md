download MacTex pkg

https://tug.org/mactex/mactex-download.html



Add to environment 

```/Library/TeX/texbin```



![image-20221226165318676](/Volumes/disk2/notebooks/assets/image-20221226165318676.png)

```json
{
     // LaTeX
    // 不在保存的时候自动编译
    "latex-workshop.latex.autoBuild.run": "never",
    // 编译工具
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
  // 编译命令
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ],
        },
        {
            "name": "xelatex*2",
            "tools": [
                "xelatex",
                "xelatex"
            ],
        },
        {
            "name": "pdflatex",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "xe->bib->xe->xe",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdf->bib->pdf->pdf",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
    "latex-workshop.latex.autoClean.run": "onBuilt",
    "latex-workshop.latex.clean.fileTypes": [  //设定清理文件的类型  
        // "*.aux",  
        "*.bbl",  
        "*.blg",  
        "*.idx",  
        "*.ind",  
        "*.lof",  
        "*.lot",  
        // "*.out",  
        "*.toc",  
        "*.acn",  
        "*.acr",  
        "*.alg",  
        "*.glg",  
        "*.glo",  
        "*.gls",  
        "*.ist",  
        "*.fls",  
        "*.log",  
        "*.fdb_latexmk",  
        "*.nav",  
        "*.snm",  
        "*.synctex.gz"  
    ],
```





```c++
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}

int Egcd(int a, int b, int *x, int *y) {
    int result, t;
    // 递归终止条件
    if (0 == b) {
        *x = 1, *y = 0;
        return a;
    }
    result = Egcd(b, a % b, x, y);
    t = *x, *x = *y, *y = t - a / b * (*y);
    return result;
}

int Inverse(int a, int b){
    int x = 0, y = 0;
    if (Egcd(a, b, &x, &y) != 1)
        return -1;
    // 确保余数与被除数符号一致
    if (b < 0)
        a = -a;
    if ((y %= a) <= 0)
        y += a;
    return y;
}

		typedef unsigned long long ll;
		ll p = 61, q = 53;
    ll n = p * q;
    ll e = 17, d;
    ll phi = Eular(n);
    do{
        while(gcd(phi, e) != 1) e++;
        d = Inverse(phi, e);
    }while(d == -1);

    printf("%d %d \n", n, e);
    printf("%d %d \n", n, d);


unsigned long long int modpow(int base, int power, int mod) {
    int i;
    unsigned long long int result = 1;
    for (i = 0; i < power; i++) {
        result = (result * base) % mod;
    }
    return result;
}
```

