    #S:
    import socket
    from threading import Thread
    import time
    
    
    def thread_leer_archivo(nombre_archivo : str):
        with open (nombre_archivo, "r") as f:
            contenido = f.read()
        t1.result = contenido
    
    
    def enviar_filas(arr_listas):
        i = 1
        while True:
            if arr_listas[i] == "":
                break
            client_socket.sendall(arr_listas[i].encode())
            print(f"Envié: {arr_listas[i]}")
            time.sleep(1)
            i = i + 1 
    
    
    
    if __name__ == '__main__':
    
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
        server_address = ('localhost', 5000)
        sock.bind(server_address)
        print("Iniciando el servidor...")
        sock.listen()
        print("Esperando conexiones...")
    
        while True:
            client_socket, client_addres = sock.accept()
    
            print("Conexion establecida con cliente")
    
            try:
    
                t1 = Thread(target = thread_leer_archivo, args = ("PartesDeElectrónica.csv",))
                t1.start()
                t1.join()
                contenido = t1.result
                lineas = contenido.split("\n")
    
                t2 = Thread(target = enviar_filas, args = (lineas,))
                t2.start()
                t2.join()
                
    
               
    
            except KeyboardInterrupt:
                client_socket.close()
                break 
    
    
    
    
            finally:
                client_socket.close()
                break



          #C:

          import socket
          from threading import Thread
          
          def thread_recv_datos():
              arr = []
          
              while True:
          
                  data_b = sock.recv(1024)
                  data = data_b.decode()
                  if data == "":
                      break
                  print(f"Recibí: {data}")
                  arr.append(data)
          
              t1.result = arr
          
              
          def thread_process(arr):
              i = 0
              while True: 
          
                  datos_lista = arr[i].split(";")
                  print(f"-----------------Nombre: {datos_lista[1]}------------------")
          
                  costo_total = float(datos_lista[5])*float(datos_lista[3])
                  print(f"Costo total: {costo_total}")
          
                  if costo_total < 25 :
                      print(f"Clasificacion por costo: Costo bajo")
                  elif 25 <= costo_total <49.9:
                      print(f"Clasificacion por costo: Costo regular")
                  elif 50 <= costo_total < 74.9:
                      print(f"Clasificacion por costo: Costo alto")
                  else:
                      print(f"Clasificacion por costo: Costo elevado")
          
              
          
          
                  if i==3:
                      break
          
                  i = i + 1 
          
          
          
          if __name__ == '__main__':
          
              sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
              server_address = ('localhost', 5000)
          
              sock.connect(server_address)
              print("Conexion establecida con el servidor")
          
              try:
          
                  t1 = Thread(target = thread_recv_datos, args = ())
                  t1.start()
                  t1.join()
                  arr = t1.result
          
                  t2 = Thread(target = thread_process, args = (arr,))
                  t2.start()
                  t2.join()
          
          
              finally:
                  sock.close()
                  print("Conexion cerrada con el servidor.")
