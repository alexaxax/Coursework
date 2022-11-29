import matplotlib.pyplot as plt
import numpy as np

matrix = np.loadtxt("data.txt")

def two_points():
    a2, a1 = [matrix[0, 1], 0], [0, 0]
    plt.plot([a1[0], a2[0]], [a1[1], a2[1]], 'ro-')
    print(f'A1 = ({a1[0]}, {a1[1]})')
    print(f'A2 = ({a2[0]}, {a2[1]})')
    return a2, a1

def three_points():
    a2, a1 = two_points()
    x3 = (-matrix[1, 2] * matrix[1, 2] + matrix[0, 2] * matrix[0, 2] + matrix[0, 1] * matrix[0, 1]) / (2 * matrix[0, 1])
    y3 = (matrix[0, 2] * matrix[0, 2] - x3*x3) ** 0.5
    a3 = [x3, y3]
    plt.plot([a1[0], a3[0]], [a1[1], a3[1]], 'ro-')
    plt.plot([a2[0], a3[0]], [a2[1], a3[1]], 'ro-')
    print(f'A3 = ({a3[0]}, {a3[1]})')
    return a3, a2, a1

def four_points():
    a3, a2, a1 = three_points()
    c = (matrix[2, 3] ** 2 - a3[0] ** 2 - a3[1] ** 2 - matrix[0, 3] ** 2) / -2
    y4_1 = (2 * a3[1] * c + (4 * c ** 2 * a3[1] ** 2 - 4 * (a3[1] ** 2 + a3[0] ** 2) * (c ** 2 - matrix[0, 3] ** 2 * a3[0] ** 2)) ** 0.5) / (2 * (a3[1] ** 2 + a3[0] ** 2))
    x4_1 = (c - y4_1 * a3[1]) / a3[0]
    y4_2 = (2 * a3[1] * c - (4 * c ** 2 * a3[1] ** 2 - 4 * (a3[1] ** 2 + a3[0] ** 2) * (c ** 2 - matrix[0, 3] ** 2 * a3[0] ** 2)) ** 0.5) / (2 * (a3[1] ** 2 + a3[0] ** 2))
    x4_2 = (c - y4_2 * a3[1]) / a3[0]
    dist1 = ((x4_1 - a2[0]) ** 2 + (y4_1 - a2[1]) ** 2) ** 0.5
    dist2 = ((x4_2 - a2[0]) ** 2 + (y4_2 - a2[1]) ** 2) ** 0.5
    dist1 = float('{:.7f}'.format(dist1))
    dist2 = float('{:.7f}'.format(dist2))
    d24 = float('{:.7f}'.format(matrix[1, 3]))
    if dist1 == d24:
        a4 = [x4_1, y4_1]
    elif dist2 == d24:
        a4 = [x4_2, y4_2]
    print(f'A4 = ({a4[0]}, {a4[1]})')
    plt.plot([a1[0], a4[0]], [a1[1], a4[1]], 'ro-')
    plt.plot([a2[0], a4[0]], [a2[1], a4[1]], 'ro-')
    plt.plot([a3[0], a4[0]], [a3[1], a4[1]], 'ro-')
    return a4, a3, a2, a1

n = int(input('Сколько точек вы хотите изобразить? '))
if n == 2:
    two_points()

elif n == 3:
    three_points()

elif n == 4:
    four_points()

plt.grid()
plt.show()