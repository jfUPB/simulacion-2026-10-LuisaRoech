# Unidad 8

## Bitácora de proceso de aprendizaje

### Explorar contextos profesionales y herramientas (Actividad 01)

Durante el desarrollo de estas actividades he tratado de explorar e indagar más este ambito dentro de blender. Mi enfasis es videojuegos, pero siempre me ha apasionado todo el enfoque artistico que esta dentro de estos: encontrar un punto intermedio entre lo programativo y el arte, no tanto como el arte generativo, sino algo más controlable e intencional. Blender ha sido una de mis aplicaciones favoritas para explorar el entorno 3D, el hecho de que sea una herramienta de código abierto y ampliamente utilizado en distintas industrias creativas hace que tenga una comunidad muy activa y una gran cantidad de posibilidades de experimentar.

Por esto me gustaria enfocarme en transferir todo lo aprendido en blender, este me permite unir procesos técnicos y visuales dentro de un mismo flujo de trabajo. Además, Blender tiene un ecosistema muy fuerte para este tipo de exploración procedural gracias a herramientas como Geometry Nodes, que funcionan mediante lógica basada en nodos y permiten generar comportamientos complejos sin depender únicamente de código tradicional. Esto hace posible experimentar con sistemas dinámicos, crecimiento orgánico, distribución procedural y simulaciones físicas desde una perspectiva mucho más artística y espacial.

<img width="1600" height="800" alt="image" src="https://github.com/user-attachments/assets/dae0cb59-2aba-4bd7-bb21-6eb1a9bb8114" />

Ya cuando hablamos de referentes, **_Ian Hubert_** suele ser de los más reconocibles, pues su trabajo mezcla entornos 3D, procesos rápidos y soluciones experimentales que priorizan el impacto visual sobre la complejidad técnica, me gusta mucho que utilice herramientas generativas como un medio no un fin en sí mismo, es una intención visual detras de los sistemas procedurales.

También se encuentra **_Ash Thorp_**, un artista que trabaja entre diseño conceptual, cine y videojuegos, utilizando herramientas 3D y workflows procedurales para contruir escenas futuristas y cinematográficas. Lo que más me interesa es la forma en que combina sistemas técnicos  con diseño visual altamente intencional. Sus entornos y composiciones tienen una estética muy cuidada, pero detrás existen procesos modulares, simulaciones y construcción procedural de elementos visuales.

Ahora bien, para mi pieza final, considero dos posibles contextos profesionales. El primero sería el desarrollo visual para videojuegos independientes, especialmente en la creación de entornos dinámicos o escenas que reaccionen a sistemas físicos o comportamientos emergentes. El segundo sería el contexto de instalaciones digitales o experiencias audiovisuales interactivas, donde los principios de Nature of Code puedan convertirse en visuales inmersivos en tiempo real, combinando arte generativo, simulación y diseño espacial. Ambos contextos me interesan porque permiten integrar programación, sistemas visuales y dirección artística dentro de una misma experiencia.

<img width="1920" height="1006" alt="image" src="https://github.com/user-attachments/assets/9e8c07b5-31ff-41e3-a91f-02591e78fd57" />

### Elegir el sistema que vas a transferir (Actividad 02)

Flow fields, es lo que principalmente quiero manejar en conjunto con la aleatoriedad y comportamientos de agentes. En p5.js, este sistema funcionaba mediante un campo vectorial que definía la dirección de movimiento en distintas zonas del espacio. Los agentes —en mi caso representados como pequeñas “hormigas” en la unidad 6— recorrían el entorno siguiendo esos vectores, generando trayectorias fluidas y naturales. Además, el comportamiento estaba influenciado por elementos aleatorios y por el ritmo de una canción, lo que hacía que el movimiento cambiara dependiendo de la intensidad y atmósfera del audio. El resultado era una especie de ecosistema dinámico donde múltiples partículas reaccionaban simultáneamente dentro de una composición visual.

Y quiero transferir este sistema a Blender porque me interesa explorar, en lugar de enfocarme únicamente en visualizar partículas abstractas, quiero construir una escena con intención narrativa y atmosférica. Blender me permite trabajar iluminación, materiales, cámara, profundidad y composición espacial, haciendo posible que los sistemas generativos no solo funcionen técnicamente, sino que también ayuden a construir una experiencia emocional y visual más fuerte.

La pieza visual que imagino construir está relacionada con el trabajo que realice en la **Unidad 6**, el cual buscaba transmitir la sensación de un hogar abandonado donde todavía permanece una especie de nostalgia o memoria de quienes habitaron el lugar. La idea es representar un espacio vacío pero que aún conserve rastros de presencia humana. Dentro de ese entorno existirían pequeñas entidades o “hormigas” que funcionarían como los nuevos habitantes del hogar, moviéndose mediante flow fields.

Me interesa que el movimiento de estos agentes no se vea completamente aleatorio, sino orgánico e intencional, como si el espacio todavía tuviera una energía o memoria invisible guiando sus recorridos. 

Ya hablando de las dificultades técnicas que anticipo, una de las principales es encontrar la forma de implementar flow fields y comportamiento de agentes dentro de Blender, especialmente usando Geometry Nodes o simulaciones. También considero complejo lograr sincronización entre audio y movimiento procedural en tiempo real, ya que requerirá traducir información sonora en cambios visuales coherentes.

### Investigar los equivalentes técnicos en la nueva herramienta (Actividad 03)

Dentro de geometry nodes existe un espacio llamado "Simulation Zone". Este sistema permite mantener información entre frames, lo que hace posible simular movimiento, acumulación y comportamiento dinámico de agentes dentro del espacio 3D. A diferencia de otros nodos más estáticos, las Simulation Zones permiten construir comportamientos continuos en el tiempo, algo fundamental para recrear sistemas similares a flow fields o partículas autónomas.

También necesito profundizar en el manejo de vectores, ruido procedural, instancias y atributos dentro de Geometry Nodes, ya que estos elementos son esenciales para controlar dirección, velocidad y comportamiento de múltiples agentes al mismo tiempo. Además, me interesa aprender formas de conectar audio o información sonora con parámetros visuales para que el movimiento pueda reaccionar al ritmo y atmósfera de la música.

**PRUEBA 1**

La primera prueba técnica consistió en explorar cómo mover múltiples instancias dentro de una Simulation Zone. La idea era comprobar si podía recrear un sistema básico de agentes que conservara información de posición y movimiento entre frames.

<img width="1574" height="851" alt="Captura de pantalla 2026-05-19 142722" src="https://github.com/user-attachments/assets/fbb981fa-010a-4838-9e5d-53c82c466f61" />

<img width="1555" height="666" alt="Captura de pantalla 2026-05-19 142811" src="https://github.com/user-attachments/assets/31da2fbb-75e5-4b38-98a2-ac9efa461a8d" />


import bpy
from bpy_extras import view3d_utils
from mathutils import Vector
from mathutils.geometry import intersect_line_plane

class MouseFollowerOperator(bpy.types.Operator):
    bl_idname = "wm.mouse_follower"
    bl_label = "Mouse Follower"

    def modal(self, context, event):
        if event.type == 'MOUSEMOVE':

            # Buscar un VIEW_3D
            for area in context.screen.areas:
                if area.type == 'VIEW_3D':
                    region = area.regions[-1]
                    rv3d = area.spaces.active.region_3d
                    break
            else:
                return {'PASS_THROUGH'}

            coord = (event.mouse_x - region.x, event.mouse_y - region.y)

            view_vector = view3d_utils.region_2d_to_vector_3d(region, rv3d, coord)
            ray_origin = view3d_utils.region_2d_to_origin_3d(region, rv3d, coord)

            # Plano Z = 0
            plane_point = Vector((0, 0, 0))
            plane_normal = Vector((0, 0, 1))

            location = intersect_line_plane(
                ray_origin,
                ray_origin + view_vector * 1000,
                plane_point,
                plane_normal
            )

            obj = bpy.data.objects.get("Mouse_Control")
            if obj and location:
                obj.location = location

        if event.type in {'ESC'}:
            return {'CANCELLED'}

        return {'PASS_THROUGH'}

    def invoke(self, context, event):
        context.window_manager.modal_handler_add(self)
        return {'RUNNING_MODAL'}

def register():
    bpy.utils.register_class(MouseFollowerOperator)

def unregister():
    bpy.utils.unregister_class(MouseFollowerOperator)

if __name__ == "__main__":
    register()
    bpy.ops.wm.mouse_follower('INVOKE_DEFAULT')
 

**PRUEBA 2**

Este más acercado a lo que quiero realizar, es un funcionamiento básico de 



### Estudio de transferencia (Actividad 04)


## Bitácora de aplicación 


## Bitácora de reflexión

a
