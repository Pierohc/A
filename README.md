    import sys
    import time
    
    def f(x):
        sys.set_int_max_str_digits(100000000)  # Aumenta el límite de dígitos para la conversión de números enteros a cadenas
        result = 0
        power = 1
        for i in range(1, 10001):
            result += i * (x ** power)
            power += 1
        return result
    
    
    if __name__ == '__main__':
    
        inicio = time.perf_counter()
        result = f(2023)
        final = time.perf_counter()
        #print(result)
        print(f"El tiempo de ejecución es: {final - inicio} segundos")
    
    
    
    
    
    import multiprocessing
    import time
    import sys
    
    
    def calculate_term(term, x):
        return term * (x ** term)
    
    def f_parallel(x):
        sys.set_int_max_str_digits(100000000)
        terms = range(1, 10001)
        pool = multiprocessing.Pool(processes=4)
        results = pool.starmap(calculate_term, [(term, x) for term in terms])
        pool.close()
        pool.join()
        return sum(results)
    
    
    
    if __name__ == '__main__':
        inicio = time.perf_counter()
        result_parallel = f_parallel(2023)
        final = time.perf_counter()
        #print(result_parallel)
        print(f"El tiempo de ejecución es: {final-inicio} segundos")
