```
def move(strs, i, j):  #exchange the palces
    num1 = list(strs)  #letters in 'str' cannot be exchanged, change to 'list' to exchange
    num1[j+1], num1[i+1] = num1[i+1], num1[j+1]
    num1[j], num1[i] = num1[i], num1[j]
    strs = "".join(num1)  #turn back to 'str'
    return strs

def searching(temlist):  #BFS 
    global count  #get the number of searching turns
    strlist.clear()  #clear the upper level's points, to storage the points in next depth of tree
    for point in range(len(temlist)):
        strs = str(temlist[point])  #get the upper level's points one by one, turn to 'str'
        for i in range(2 * n + 1):
            j = strs.index("*")  #get the palce of '**'
            if strs[i] == "*" or strs[i+1] == "*":  #skip the pair of letters cannot be exchanged
                continue
            else :
                temstrs = move(strs, i, j)  
                count = count + 1  #searching turns +1
                if temstrs == true1 or temstrs == true2 or temstrs == true3 or temstrs == true4:
                    allin.add(temstrs)
                    goal[temstrs] = strs
                    strlist.append(temstrs)
                    return True
                if temstrs not in allin:
                    allin.add(temstrs)
                    goal[temstrs] = strs
                    strlist.append(temstrs)
    return False

def confirm(keys): #use dict to output the router
    if goal[keys] != 'root':
        result.append(goal[keys])
        confirm(goal[keys])
    else :
        return result

base = '12'
n = int(input("please input the number of repetitions of '12': " ))
root = base * n + '**'  #get start str
true1 = '1' * n + '2' * n + '**'
true2 = '**'+ '1' * n + '2' * n
true3 = '2' * n + '1' * n + '**'
true4 = '**' + '2' * n + '1' * n  #goals
count = 0  #used to record searching turns
strlist = []  #the all points in every depth, to get length of points in every depth 
strlist.append(root)  #the ponit in depth 0
allin = set()  #the set of all points, use to cut the same as father point in children point, reduce the calculation
allin.add(root)
goal = {}  #'dict' for get the pair of children:father, use to get the router
goal[root] = "root"
for depth in range(1, 10 * n):  #depth of tree, as big as possible
    l = len(strlist)  #length of points in every depth
    for m in range(l):  #the same list as every depth, to get the points in next depth
        temlist = []
        temlist += strlist
    if searching(temlist):  #start searching
        break
result = []
result.append(strlist[-1])  #get the last str=the goal
confirm(strlist[-1])
result.reverse()  
print("最小変換回数：", depth)
print("変換方法：", result)
print("探索回数：", count)

```
