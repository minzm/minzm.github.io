---
layout: single
title:  "Strassen Algorithm"
date:   2021-04-28 16:20:35 +0900
categories: jekyll update
---
1.Strassen Algorithm이란?
================
Strassen Algorithm은 독일의 수학자 Volker Strassen이 1969년에 개발한 행렬 곱셈 알고리즘이다.
행렬을 가로세로 반씩 쪼개 4등분해서 곱하면, 단순하게 할 경우 작은 행렬의 곱셈이 8번 필요하므로 복잡도는 그대로입니다.
하지만 이를 조금 더 많은 덧셈을 동원하여 바꾸어 7번의 곱셈만으로 수행할 수 있다는 것이 Strassen Algorithm의 핵심이다.

2. Algorithm 설명
============
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

코드(JAVA)
=======

{% highlight ruby %}
import java.util.Scanner;                                       // Scanner를 사용하기 위한 import 문

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
        int n = A.length;                                       // n은 배열 A의 길이
        int[][] G = new int[n][n];                              // 2차원 배열 생성 및 선언

        if (n == 1)                                             // 크기가 1이면 그냥 곱하면 된다
            G[0][0] = A[0][0] * B[0][0];
        else
        {
            int[][] A11 = new int[n/2][n/2];                    // 2차원 배열 A11 생성 (행렬 A의 첫 번째 부분)
            int[][] A12 = new int[n/2][n/2];                    // 2차원 배열 A12 생성 (행렬 A의 두 번째 부분)
            int[][] A21 = new int[n/2][n/2];                    // 2차원 배열 A21 생성 (행렬 A의 세 번째 부분)
            int[][] A22 = new int[n/2][n/2];                    // 2차원 배열 A22 생성 (행렬 A의 네 번째 부분)

            int[][] B11 = new int[n/2][n/2];                    // 2차원 배열 B11 생성 (행렬 B의 첫 번째 부분)
            int[][] B12 = new int[n/2][n/2];                    // 2차원 배열 B12 생성 (행렬 B의 두 번째 부분)
            int[][] B21 = new int[n/2][n/2];                    // 2차원 배열 B21 생성 (행렬 B의 세 번째 부분)
            int[][] B22 = new int[n/2][n/2];                    // 2차원 배열 B22 생성 (행렬 B의 네 번째 부분)


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
        Scanner scan = new Scanner(System.in);                        // Scanner 객체 생성
        System.out.println("Strassen Algorithm\n");

        Strassen s = new Strassen();                                  // Strassen 객체 생성

        System.out.println("n을 입력하세요 :");
        int N = scan.nextInt();                                       // 키 입력 부분

        System.out.println("A 행렬을 생성하세요\n");
        int[][] A = new int[N][N];                                    // 행렬 생성
        for (int i = 1; i < N + 1; i++)                                   // 입력한 크기에 맞는 행렬을 생성 후
            for (int p = 1; p < N + 1; p++)                               // 차례대로 원소들을 입력 해주게 하는 for 문
                A[i][p] = scan.nextInt();                             // 행렬 A를 입력하여 생성

        System.out.println("B 행렬을 생성하세요\n");
        int[][] B = new int[N][N];                                    // 위와 동일
        for (int i = 1; i < N + 1; i++)
            for (int p = 1; p < N + 1; p++)
                B[i][p] = scan.nextInt();

        int[][] C = s.multiply(A, B);                                 // Strassen 객체 s에 결과행렬 C 생성

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




	