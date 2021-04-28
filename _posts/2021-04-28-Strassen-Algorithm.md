---
layout: single
title:  "Strassen Algorithm"
date:   2021-04-28 16:20:35 +0900
categories: jekyll update
---
1.Strassen Algorithm이란?
================
Strassen Algorithm은 독일의 수학자 Volker Strassen이 1969년에 개발한 행렬 곱셈 알고리즘이다.


Algorithm 설명
---------------------

1 단계 : A, B, C를 가정하기 위해 3 개의 행렬을 만들고 여기서 C는 결과 행렬이고 A와 B는 Strassen의 방법을 사용하여 곱해질 행렬이다.

	A = {A11 A12 A21 A22}, B = {B11 B12 B21 B22}, C = {C11 C12 C21 C22}

2 단계 : 새로운 행렬 M을 가정한다.
	
	M1 = (A11 + A22)(B11 + B22)
	M2 = (A21 + A22) B11
	M3 = A11 (B12 - B22)
	M4 = A22 (B21 - B11)
	M5 = (A11 + A12) B22
	M6 = (A21 - A11) (B11 + B12)
	M7 = (A12 - A22) (B21 + B22)

3 단계 : 위의 식을 이용하여 행렬 C를 정리한다.

	C11 = M1 + M4 − M5 + M7 
	C12 = M3 + M5 
	C21 = M2 + M4 
	C22 = M1 − M2 + M3 + M6

2.코드(JAVA)
=======

{% highlight ruby %}
import java.util.Scanner; 

public class Strassen
{
    /** 두 행렬을 빼는 함수 **/
    public int[][] sub(int[][] A, int[][] B)
    {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int p = 0; p < n; p++)
                C[i][p] = A[i][p] - B[i][p];
        return C;
    }

    /** 두 행렬을 더하는 함수 **/
    public int[][] add(int[][] A, int[][] B)
    {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int p = 0; p < n; p++)
                C[i][p] = A[i][p] + B[i][p];
        return C;
    }

    /** 행렬을 곱하는 함수 **/
    public int[][] multiply(int[][] A, int[][] B)
    {
        int n = A.length;  
        int[][] G = new int[n][n];  

        if (n == 1)  
            G[0][0] = A[0][0] * B[0][0];
        else
        {
            int[][] A11 = new int[n/2][n/2};
            int[][] A12 = new int[n/2][n/2]; 
            int[][] A21 = new int[n/2][n/2];
            int[][] A22 = new int[n/2][n/2]; 

            int[][] B11 = new int[n/2][n/2]; 
            int[][] B12 = new int[n/2][n/2];  
            int[][] B21 = new int[n/2][n/2];
            int[][] B22 = new int[n/2][n/2];  


            /**
             새로운 행렬 M
             M1 = (A11 + A22)(B11 + B22)
             M2 = (A21 + A22) B11
             M3 = A11 (B12 - B22)
             M4 = A22 (B21 - B11)
             M5 = (A11 + A12) B22
             M6 = (A21 - A11) (B11 + B12)
             M7 = (A12 - A22) (B21 + B22)
             **/

            int [][] M1 = multiply(add(A11, A22), add(B11, B22));
            int [][] M2 = multiply(add(A21, A22), B11);
            int [][] M3 = multiply(A11, sub(B12, B22));
            int [][] M4 = multiply(A22, sub(B21, B11));
            int [][] M5 = multiply(add(A11, A12), B22);
            int [][] M6 = multiply(sub(A21, A11), add(B11, B12));
            int [][] M7 = multiply(sub(A12, A22), add(B21, B22));

            /**
             위의 식을 이용하여 행렬 C를 정리
             C11 = M1 + M4 - M5 + M7
             C12 = M3 + M5
             C21 = M2 + M4
             C22 = M1 - M2 + M3 + M6
             **/

            int [][] C11 = add(sub(add(M1, M4), M5), M7);
            int [][] C12 = add(M3, M5);
            int [][] C21 = add(M2, M4);
            int [][] C22 = add(sub(add(M1, M3), M2), M6);

        }
        return G;
    }

    /** Main 함수 시작 **/
    public static void main (String[] args)
    {
        Scanner scan = new Scanner(System.in);                       
        System.out.println("Strassen Algorithm\n");

        Strassen s = new Strassen();                                
        System.out.println("n을 입력하세요 :");
        int N = scan.nextInt();                                       

        System.out.println("A 행렬을 생성하세요\n");
        int[][] A = new int[N][N];                                    
        for (int i = 1; i < N + 1; i++)              
            for (int p = 1; p < N + 1; p++)                               
                A[i][p] = scan.nextInt();                             

        System.out.println("B 행렬을 생성하세요\n");
        int[][] B = new int[N][N];                                    
        for (int i = 1; i < N + 1; i++)
            for (int p = 1; p < N + 1; p++)
                B[i][p] = scan.nextInt();

        int[][] C = s.multiply(A, B);                                 

        System.out.println("행렬 A와 B의 곱 : ");
        for (int i = 1; i < N + 1; i++)
        {
            for (int p = 1; p < N + 1; p++)
                System.out.print(C[i][p] +" ");
            System.out.println();
        }

    }
}
{% endhighlight %}


3. Strassen Algorithm 성능
==================
두 행렬을 곱하는 것보다 이 알고리즘을 사용했을 경우 더 적은 시간이 소요된다. 하지만 Strassen Algorithm은 속도에 비해 수치 안정성이 떨어지는 것으로 알려져있따.
때문에 성능적인 면에서 이 알고리즘은 속도는 빠르지만 실제 오차가 일반적인 행렬 곱셈보다 더 크다.




	