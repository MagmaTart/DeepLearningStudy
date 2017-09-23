# ��İ��� �Ű��

������ ���� �Ű���� �ִٰ� ��������.

![](image/Network.png)

�� �Ű�� �׷�������, ������ ��ġ�� Weight(����ġ)�� �ǹ��Ѵ�. �Է��� 2��, ����� ��尡 3���� ��Ʈ��ũ�̹Ƿ�, ���� �������� ���̵��� Weight�� �� 6���� ���� �����ϰ� �ȴ�.

�̸� ���ݱ��� �����ؿԴ� �ۼ�Ʈ���� ���·� �����غ���.

```
def pctn(x1, x2):
    y1 = 1*x1 + 2*x2
    y2 = 3*x1 + 4*x2
    y3 = 5*x1 + 6*x2
    return y1, y2, y3

print(pctn(3, 5))
```

����� ������ ����, �� ���� ��� ����� �� �����ְ� �ִ� ���� �� �� �ִ�.

```
(13, 29, 45)
```

������ �̷��� ���� ������ �ϰ� �Ǹ�, ��尡 �������� ���̾ ������� �ڵ��� ���̰� ���ϱ޼������� �þ ���̴�. �׷��� ��ķ� �� ������ ������ ������ �� �ִ�.

�Է� ��� ![eq](https://latex.codecogs.com/png.latex?X%20%3D%20%5Cbegin%7Bpmatrix%7D%20x_1%20%5C%20x_2%20%5Cend%7Bpmatrix%7D) �� ����ġ ��� ![eq](https://latex.codecogs.com/png.latex?X%20%3D%20%5Cbegin%7Bpmatrix%7D%20w_1_1%20%5C%20w_2_1%20%5C%20w_3_1%20%5C%5C%20w_1_2%20%5C%20w_2_2%20%5C%20w_3_2%20%5Cend%7Bpmatrix%7D) �� ���Ͽ� ��� ![eq](https://latex.codecogs.com/png.latex?Y%20%3D%20%5Cbegin%7Bpmatrix%7D%20y_1%20%5C%20y_2%20%5C%20y_3%20%5Cend%7Bpmatrix%7D) �� ���� �� �ִ�. ��� ������ ���� �ڼ��� ������ �� ![��ũ](https://ko.wikipedia.org/wiki/%ED%96%89%EB%A0%AC_%EA%B3%B1%EC%85%88) �� ��������.

��İ��� ������ ����, ���� �Ű������ �� Weight���� ���� ��ķ� ��Ÿ����, ![eq](https://latex.codecogs.com/png.latex?W%20%3D%20%5Cbegin%7Bpmatrix%7D%201%20%5C%203%20%5C%205%20%5C%5C%202%20%5C%204%20%5C%206%20%5Cend%7Bpmatrix%7D) ���� ��Ÿ�� �� �ִ�. 

���� ó���� �Ű���� ��İ��� ���� ����ġ ��� ![eq](https://latex.codecogs.com/png.latex?W)�� �̿��Ͽ� ������ ����.

![eq](https://latex.codecogs.com/png.latex?%5Cbegin%7Bpmatrix%7D%20x_1%20%5C%20x_2%20%5Cend%7Bpmatrix%7D%20%5Ccdot%20%5Cbegin%7Bpmatrix%7D%20w_1_1%20%5C%20w_2_1%20%5C%20w_3_1%20%5C%5C%20w_1_2%20%5C%20w_2_2%20%5C%20w_3_2%20%5Cend%7Bpmatrix%7D%20%3D%20%5Cbegin%7Bpmatrix%7D%20y_1%20%5C%20y_2%20%5C%20y_3%20%5Cend%7Bpmatrix%7D)

```
import numpy as np

def pctn(X):
    W = np.array([[1, 3, 5], [2, 4, 6]])
    return np.dot(X, W)

X = np.array([3, 5])
print(pctn(X))
```

`np.dot()` �Լ��� ��İ��� �����մϴ�. ����� ������ �� ������ ���� �� �� �ֽ��ϴ�.
```
[13, 29, 45]
```

������ ����, ���� ��İ��� ������ �����ϰ�, �����ϴ� ������ ���� ���� ����� ������ �����ϴٴ� ���̴�. ![eq](https://latex.codecogs.com/png.latex?%28a%5Ctimes%20b%29) ũ�� ��� ![eq](https://latex.codecogs.com/png.latex?X) �� ![eq](https://latex.codecogs.com/png.latex?%28c\times%20d%29) ũ�� ��� ![eq](https://latex.codecogs.com/png.latex?W)�� ������ ![eq](https://latex.codecogs.com/png.latex?Y%3DX%5Ccdot%20W) ����� ũ��� ![eq](https://latex.codecogs.com/png.latex?%28a%5Ctimes%20d%29) �̴�. �̶� ![eq](https://latex.codecogs.com/png.latex?b%20%5Cneq%20c) �̸�, ��� ������ �� �� ����.