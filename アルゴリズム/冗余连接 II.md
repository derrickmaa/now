from collections import Counter
class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        def cyc(graph, value):
            child = collections.defaultdict(list)
            for u, v in graph:
                child[v].append(u)
            p = child[value][0]
            while True:
                if value != p:
                    if p in child:
                        p = child[p][0]
                    else:
                        return False
                else:
                    return True
                
                
        n = len(edges)
        nodev = None
        parent = collections.defaultdict(list)
        child = collections.defaultdict(list)
        for u, v in edges:
            parent[v].append(u)
            child[u].append(v)
            if len(parent[v]) > 1:
                nodev = v
            if len(child[u]) > 1: 
                nodep = u
        
        if nodev != None:
            place =[y for x, y in edges]
            t1 = place.index(nodev)
            tem = place[:]
            tem.remove(nodev)
            t2 = tem.index(nodev) + 1
            temedges = edges[:]
            del temedges[t2]
            if cyc(temedges, nodev):
                return edges[t1]
            else:
                return edges[t2]
        if nodev == None:
            for u, v in edges:
                if v == nodep:
                    return[u, v]