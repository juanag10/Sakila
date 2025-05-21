# Sakila

package com.sakila.views;

import com.sakila.controllers.ActorController;
import com.sakila.models.Actor;

import java.util.List;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        ActorController actorController = new ActorController();
        Scanner sc = new Scanner(System.in);
        boolean salir = false;

        while (!salir) {
            System.out.println("\n--- Gestión de Actores ---");
            System.out.println("1. Listar actores");
            System.out.println("2. Agregar actor");
            System.out.println("3. Actualizar actor");
            System.out.println("4. Eliminar actor");
            System.out.println("5. Salir");
            System.out.print("Elija opción: ");

            int opcion = sc.nextInt();
            sc.nextLine(); // limpiar buffer

            switch (opcion) {
                case 1:
                    List<Actor> actores = actorController.obtenerActores();
                    System.out.println("\nLista de actores activos:");
                    for (Actor a : actores) {
                        System.out.println(a.getActorId() + ": " + a.getFirstName() + " " + a.getLastName());
                    }
                    break;

                case 2:
                    System.out.print("Nombre: ");
                    String nombre = sc.nextLine();
                    System.out.print("Apellido: ");
                    String apellido = sc.nextLine();
                    Actor nuevo = new Actor(0, nombre, apellido, true);
                    if (actorController.agregarActor(nuevo)) {
                        System.out.println("Actor agregado.");
                    } else {
                        System.out.println("Error al agregar actor.");
                    }
                    break;

                case 3:
                    System.out.print("ID actor a actualizar: ");
                    int idActualizar = sc.nextInt();
                    sc.nextLine();
                    Actor actorActualizar = actorController.buscarPorId(idActualizar);
                    if (actorActualizar != null) {
                        System.out.print("Nuevo nombre (" + actorActualizar.getFirstName() + "): ");
                        String nuevoNombre = sc.nextLine();
                        if (!nuevoNombre.isEmpty()) {
                            actorActualizar.setFirstName(nuevoNombre);
                        }
                        System.out.print("Nuevo apellido (" + actorActualizar.getLastName() + "): ");
                        String nuevoApellido = sc.nextLine();
                        if (!nuevoApellido.isEmpty()) {
                            actorActualizar.setLastName(nuevoApellido);
                        }
                        if (actorController.actualizarActor(actorActualizar)) {
                            System.out.println("Actor actualizado.");
                        } else {
                            System.out.println("Error al actualizar actor.");
                        }
                    } else {
                        System.out.println("Actor no encontrado.");
                    }
                    break;

                case 4:
                    System.out.print("ID actor a eliminar: ");
                    int idEliminar = sc.nextInt();
                    sc.nextLine();
                    if (actorController.eliminarActor(idEliminar)) {
                        System.out.println("Actor eliminado (marcado como inactivo).");
                    } else {
                        System.out.println("Error al eliminar actor.");
                    }
                    break;

                case 5:
                    salir = true;
                    System.out.println("Saliendo...");
                    break;

                default:
                    System.out.println("Opción inválida.");
            }
        }

        actorController.cerrar();
        sc.close();
    }
}
