# Differential-Evolution

import random
population = [(0.5974, 0.4286),(0.6019, 0.5878),(0.3999, 0.2296),(-0.1422, -0.6867),(-0.3625, -0.5348)]
popsize = 5

def func1(x):
  # Sphere function, use any bounds, f(0,...,0)=0
  return sum([x[i] ** 2 for i in range(len(x))])

cost_func = func1  # Cost function

def mainprocess():
  for j in range(0, popsize):
    print("Individual number: ",j)
    candidates = []
    for k in range(0, popsize):
      candidates.append(k)
    candidates.remove(j)
    random_index = random.sample(candidates, 3)
    print(random_index)
    x_1 = population[random_index[0]]
    x_2 = population[random_index[1]]
    x_3 = population[random_index[2]]
    x_t = population[j]  # target individual

    # subtract x3 from x2, and create a new vector (x_diff)
    x_diff = [x_2_i - x_3_i for x_2_i, x_3_i in zip(x_2, x_3)]

    # multiply x_diff by the mutation factor (F) and add to x_1
    v_donor = [x_1_i + 0.5* x_diff_i for x_1_i, x_diff_i in zip(x_1, x_diff)]
    print("v_donor is:")
    print(v_donor)
    v_trial = []
    for k in range(len(x_t)):
      crossover = random.random()
      if crossover <= 0.7:
        v_trial.append(v_donor[k])
      else:
        v_trial.append(x_t[k])
    print("v_trial:")
    print(v_trial)

    score_trial = cost_func(v_trial)
    score_target = cost_func(x_t)

    if score_trial < score_target:
      population[j] = v_trial
      print("Selecting trial vector")
      print(score_trial," > ",v_trial)

    else:
      print("Selecting target vector")
      print(score_target," > ",x_t)

mainprocess()
# gen_score = []
