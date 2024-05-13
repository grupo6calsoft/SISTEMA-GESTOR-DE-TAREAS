# SISTEMA-GESTOR-DE-TAREAS
Un sistema de gestión de tareas en línea de comandos permite crear, asignar, y actualizar tareas, generar reportes, y consultar tareas por estado o asignadas a un miembro. Simplifica la coordinación y monitoreo del trabajo en equipo.
import os
import datetime

class Tarea:
    def __init__(self, titulo, descripcion, fecha_limite):
        self.titulo = titulo
        self.descripcion = descripcion
        self.fecha_limite = datetime.datetime.strptime(fecha_limite, "%Y-%m-%d").date()
        self.estado = "Pendiente"
        self.responsables = []

    def asignar_tarea(self, miembro):
        self.responsables.append(miembro)

    def actualizar_estado(self, estado):
        self.estado = estado

    def editar_tarea(self, titulo=None, descripcion=None, fecha_limite=None):
        if titulo:
            self.titulo = titulo
        if descripcion:
            self.descripcion = descripcion
        if fecha_limite:
            self.fecha_limite = datetime.datetime.strptime(fecha_limite, "%Y-%m-%d").date()

class MiembroEquipo:
    def __init__(self, nombre):
        self.nombre = nombre
        self.tareas_asignadas = []

    def asignar_tarea(self, tarea):
        self.tareas_asignadas.append(tarea)

class GestorTareas:
    def __init__(self):
        self.tareas = []
        self.miembros_equipo = []

    def crear_tarea(self, titulo, descripcion, fecha_limite):
        tarea = Tarea(titulo, descripcion, fecha_limite)
        self.tareas.append(tarea)
        return tarea

    def asignar_tarea(self, tarea, miembro):
        tarea.asignar_tarea(miembro)
        miembro.asignar_tarea(tarea)

    def actualizar_estado_tarea(self, tarea, estado):
        tarea.actualizar_estado(estado)

    def editar_tarea(self, tarea, titulo=None, descripcion=None, fecha_limite=None):
        tarea.editar_tarea(titulo, descripcion, fecha_limite)

    def generar_reporte(self):
        print("=== Reporte de Tareas ===")
        for tarea in self.tareas:
            print(f"Título: {tarea.titulo}")
            print(f"Descripción: {tarea.descripcion}")
            print(f"Fecha Límite: {tarea.fecha_limite}")
            print(f"Estado: {tarea.estado}")
            print("Responsables:")
            for miembro in tarea.responsables:
                print(f"- {miembro.nombre}")
            print()

    def obtener_tareas_por_estado(self, estado):
        return [tarea for tarea in self.tareas if tarea.estado.lower() == estado.lower()]

    def obtener_tareas_asignadas_a_miembro(self, nombre_miembro):
        tareas_miembro = []
        for tarea in self.tareas:
            for responsable in tarea.responsables:
                if responsable.nombre == nombre_miembro:
                    tareas_miembro.append(tarea)
        return tareas_miembro

    def obtener_tareas_por_vencer(self, dias=7):
        hoy = datetime.date.today()
        tareas_por_vencer = []
        for tarea in self.tareas:
            if (tarea.fecha_limite - hoy).days <= dias:
                tareas_por_vencer.append(tarea)
        return tareas_por_vencer

    def eliminar_tarea(self, titulo):
        tarea = self.buscar_tarea_por_titulo(titulo)
        if tarea:
            self.tareas.remove(tarea)
            print("¡Tarea eliminada exitosamente!")
        else:
            print("Tarea no encontrada.")

    def agregar_miembro_equipo(self, nombre):
        miembro = MiembroEquipo(nombre)
        self.miembros_equipo.append(miembro)
        print(f"¡Miembro de equipo '{nombre}' agregado exitosamente!")

    def listar_miembros_equipo(self):
        print("=== Miembros de Equipo ===")
        for miembro in self.miembros_equipo:
            print(miembro.nombre)

    def buscar_tarea_por_titulo(self, titulo):
        for tarea in self.tareas:
            if tarea.titulo == titulo:
                return tarea
        return None

# Función para mostrar menú y manejar la interacción del usuario
def menu_principal():
    print("=== Sistema de Gestión de Tareas ===")
    print("1. Crear Tarea")
    print("2. Asignar Tarea")
    print("3. Actualizar Estado de Tarea")
    print("4. Editar Tarea")
    print("5. Generar Reportes")
    print("6. Ver Tareas por Estado")
    print("7. Ver Tareas Asignadas a Miembro")
    print("8. Ver Tareas por Vencer")
    print("9. Eliminar Tarea")
    print("10. Agregar Miembro de Equipo")
    print("11. Listar Miembros de Equipo")
    print("12. Salir")

    opcion = input("Ingrese su opción: ")
    return opcion

# Función para crear tarea con título y descripción en una sola línea
def crear_tarea_descripcion(titulo, descripcion, fecha_limite):
    tarea = gestor_tareas.crear_tarea(titulo, descripcion, fecha_limite)
    print("¡Tarea creada exitosamente!")
    return tarea

# Función para asignar tarea a miembro del equipo
def asignar_tarea_miembro(titulo, nombre_miembro):
    tarea = gestor_tareas.buscar_tarea_por_titulo(titulo)
    miembro = buscar_miembro_por_nombre(nombre_miembro)

    if tarea and miembro:
        gestor_tareas.asignar_tarea(tarea, miembro)
        print("¡Tarea asignada exitosamente!")
    else:
        print("¡Tarea o miembro no encontrado!")

# Función para actualizar estado de tarea
def actualizar_estado_tarea(titulo, estado):
    tarea = gestor_tareas.buscar_tarea_por_titulo(titulo)
    if tarea:
        gestor_tareas.actualizar_estado_tarea(tarea, estado)
        print("¡Estado de tarea actualizado exitosamente!")
    else:
        print("¡Tarea no encontrada!")

# Función para editar tarea
def editar_tarea(titulo, nuevo_titulo=None, nueva_descripcion=None, nueva_fecha_limite=None):
    tarea = gestor_tareas.buscar_tarea_por_titulo(titulo)
    if tarea:
        gestor_tareas.editar_tarea(tarea, nuevo_titulo, nueva_descripcion, nueva_fecha_limite)
        print("¡Tarea editada exitosamente!")
    else:
        print("¡Tarea no encontrada!")

# Función para generar reportes
def generar_reportes():
    gestor_tareas.generar_reporte()

# Función para ver tareas por estado
def ver_tareas_por_estado(estado):
    tareas = gestor_tareas.obtener_tareas_por_estado(estado)
    if tareas:
        print(f"Tareas con estado '{estado}':")
        for tarea in tareas:
            print(f"- {tarea.titulo}")
    else:
        print("¡No se encontraron tareas con ese estado!")

# Función para ver tareas asignadas a un miembro
def ver_tareas_asignadas_a_miembro(nombre_miembro):
    tareas = gestor_tareas.obtener_tareas_asignadas_a_miembro(nombre_miembro)
    if tareas:
        print(f"Tareas asignadas a {nombre_miembro}:")
        for tarea in tareas:
            print(f"- {tarea.titulo}")
    else:
        print(f"No se encontraron tareas asignadas a {nombre_miembro}.")

# Función para ver tareas por vencer en ciertos días
def ver_tareas_por_vencer(dias=7):
    tareas = gestor_tareas.obtener_tareas_por_vencer(dias)
    if tareas:
        print(f"Tareas por vencer en los próximos {dias} días:")
        for tarea in tareas:
            print(f"- {tarea.titulo} (Fecha Límite: {tarea.fecha_limite})")
    else:
        print("¡No hay tareas próximas a vencer!")

# Función para eliminar tarea
def eliminar_tarea(titulo):
    gestor_tareas.eliminar_tarea(titulo)

# Función para agregar miembro al equipo
def agregar_miembro_equipo(nombre):
    gestor_tareas.agregar_miembro_equipo(nombre)

# Función para listar miembros del equipo
def listar_miembros_equipo():
    gestor_tareas.listar_miembros_equipo()

# Función para buscar un miembro del equipo por nombre
def buscar_miembro_por_nombre(nombre):
    for miembro in gestor_tareas.miembros_equipo:
        if miembro.nombre == nombre:
            return miembro
    return None

gestor_tareas = GestorTareas()

if __name__ == "__main__":
    while True:
        opcion = menu_principal()

        if opcion == "1":
            titulo = input("Ingrese el título de la tarea: ")
            descripcion = input("Ingrese la descripción de la tarea: ")
            fecha_limite = input("Ingrese la fecha límite (AAAA-MM-DD): ")
            crear_tarea_descripcion(titulo, descripcion, fecha_limite)
        elif opcion == "2":
            titulo = input("Ingrese el título de la tarea: ")
            nombre_miembro = input("Ingrese el nombre del miembro del equipo: ")
            asignar_tarea_miembro(titulo, nombre_miembro)
        elif opcion == "3":
            titulo = input("Ingrese el título de la tarea: ")
            estado = input("Ingrese el nuevo estado de la tarea: ")
            actualizar_estado_tarea(titulo, estado)
        elif opcion == "4":
            titulo = input("Ingrese el título de la tarea a editar: ")
            nuevo_titulo = input("Ingrese el nuevo título (deje en blanco para mantener el actual): ")
            nueva_descripcion = input("Ingrese la nueva descripción (deje en blanco para mantener la actual): ")
            nueva_fecha_limite = input("Ingrese la nueva fecha límite (AAAA-MM-DD) (deje en blanco para mantener el actual): ")
            editar_tarea(titulo, nuevo_titulo, nueva_descripcion, nueva_fecha_limite)
        elif opcion == "5":
            generar_reportes()
        elif opcion == "6":
            estado = input("Ingrese el estado de la tarea (Pendiente, En Progreso, Completada): ")
            ver_tareas_por_estado(estado)
        elif opcion == "7":
            nombre_miembro = input("Ingrese el nombre del miembro del equipo: ")
            ver_tareas_asignadas_a_miembro(nombre_miembro)
        elif opcion == "8":
            dias = int(input("Ingrese el número de días para las tareas por vencer: "))
            ver_tareas_por_vencer(dias)
        elif opcion == "9":
            titulo = input("Ingrese el título de la tarea a eliminar: ")
            eliminar_tarea(titulo)
        elif opcion == "10":
            nombre = input("Ingrese el nombre del nuevo miembro del equipo: ")
            agregar_miembro_equipo(nombre)
        elif opcion == "11":
            listar_miembros_equipo()
        elif opcion == "12":
            print("Saliendo del programa...")
            break
        else:
            print("¡Opción inválida! Por favor, inténtelo de nuevo.")
