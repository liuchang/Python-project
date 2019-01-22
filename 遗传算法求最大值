import numpy as np
import random
import matplotlib.pyplot as plt

population_size = 50   #种群数量
pc = 0.6               #交配概率
pm = 0.01              #变异概率
n_generations = 200    #进化代数
DNA_length = 10        #染色体长度（与X共同确定精度）
x_bound = [0, 15]      # 自变量x取值范围   
#fig = plt.figure()    #

#定义函数
def F(x):
    return (-3)*np.sin(x)*(x-30)**2

class GA(object):
    #对DNA随机产生染色体
    def __init__(self):
        self.populations = np.random.randint(0, 2, (population_size, DNA_length))#随机产生DNA
 
    # 对所有DNA进行解码，翻译成10进制数字
    def DNA_decode(self, loser_winner):
        return np.dot(loser_winner, 2 ** np.arange(DNA_length)) / (2 ** DNA_length-1) * x_bound[1]#DNA二进制转译为十进制

    #计算适合度
    def calculate_fitness(self, loser_winner):
        DNA_value = self.DNA_decode(loser_winner)#转译十进制
        fitness = F(DNA_value)#计算适合度
        fitness = np.where(fitness < 0.0, 0.0, fitness)#去除掉负值
        return fitness

    #选择
    def selection(self):
        loser_winner_id = np.random.choice(np.arange(population_size), size=2, replace=False)# 从0~DNA.length-1中随机选择2组DNA编号
        loser_winner = self.populations[loser_winner_id]#提出编号对应的染色体
        loser_winner_fitness = self.calculate_fitness(loser_winner)#计算适合度
        return loser_winner[np.argsort(loser_winner_fitness)], loser_winner_id#返回排序的适合度（按照loser_winner）

    #交配
    def crossover(self, loser_winner):
        # 必须指定类型为np.bool，否则True会变为1.0000的小数
        crossover_points = np.empty((DNA_length,)).astype(np.bool)
        for i in range(DNA_length):
            crossover_points[i] = True if random.random() < pc else False
        loser_winner[0, crossover_points] = loser_winner[1, crossover_points]
        return loser_winner

    #突变     
    def mutation(self, loser_winner):
        for i in range(DNA_length):
            loser_winner[0, i] = 1 if loser_winner[0, i] == 0 else 0
        return loser_winner

    #进化(分三步)
    def evolve(self):
        for i in range(int(population_size/2)):
            loser_winner, loser_winner_id = self.selection() #（选择）
            loser_winner = self.crossover(loser_winner)     #（交配）
            loser_winner = self.mutation(loser_winner)     #(突变)
            self.populations[loser_winner_id] = loser_winner
 
 
population_value = np.linspace(x_bound[0], x_bound[1], 200)#指定间隔分两百份
plt.plot(population_value, F(population_value))

ga=GA()
a=0
print("Please Wait...")
for step in range(n_generations):
    population_fitness = ga.calculate_fitness(ga.populations)
    x = ga.DNA_decode(ga.populations[np.argmax(population_fitness)])
    y_max = np.max(population_fitness)
    if 'sca' in globals():
        sca.remove()
    sca = plt.scatter(x, y_max, s=100, c='red')
    plt.pause(0.01)
    a=a+1
    #print('X value : %.3f, Y_Max: %.3f , the order: %d' % (x, y_max,a)) 
    ga.evolve()
print('X value : %.3f, Y_Max: %.3f ' % (x, y_max)) 
plt.show()
