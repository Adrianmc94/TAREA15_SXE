# TAREA15_SXE Módulo de Odoo el cúal calcula que necesitas con tu nivel de cansancio!:
## Para empezar crearemos la estructura inicial para el módulo en la carpeta addons
### Usaremos el comando scaffold ( como root por si el usuario de Docker no tiene permisos):
'''' bash
docker exec -it -u 0 odoo18_app odoo scaffold calcular-cansancio /mnt/extra-addons
''''
### Para otergar los permisos usamos el siguiente comando:
docker exec -it odoo18_app chmod -R 777 /mnt/extra-addons/calcular-cansancio

## Ahora tenemos que configurar el archivo manifest, dentro de la carpeta addons calcular-cansancio:
<img width="1267" height="876" alt="image" src="https://github.com/user-attachments/assets/b044cb13-09ff-4255-89df-62b197a16187" />

## Seguido de la anterior configuración ahora configuramos el archivo models.py:
from odoo import models, fields, api

class CansancioRegistro(models.Model):
    _name = 'cansancio.registro'
    _description = 'Registro de Nivel de Sueño y Bebida Recomendada'

    nombre_alumno = fields.Char(
        string='Alumno',
        required=True
    )

    nivel_sueno = fields.Integer(
        string='Nivel de Sueño (1-10)',
        required=True,
        help="Introduce el nivel de sueño (1 = poco, 10 = máximo)"
    )

    bebida_recomendado = fields.Char(
        string='Bebida Recomendada',
        compute='_calcular_bebida',
        store=True, 
        readonly=True
    )

    # MÉTODO PARA CALCULAR LA BEBIDA
    @api.depends('nivel_sueno')
    def _calcular_bebida(self):
        for record in self:
            nivel = record.nivel_sueno
            bebida = ''
            
            # Reglas para calcular la bebida:
            if 1 <= nivel <= 3:
                bebida = 'Café con leche'
            elif 4 <= nivel <= 6:
                bebida = 'Café solo largo'
            elif 7 <= nivel <= 9:
                bebida = 'Café solo larguísimo'
            elif nivel == 10:
                bebida = 'Inyección de adrenalina'
            else:
                bebida = 'Nivel de sueño fuera de rango (1-10)'
            
            record.bebida_recomendado = bebida

## Por ultimo configuramos le archivo views.xml:
<?xml version="1.0" encoding="UTF-8"?>
<odoo>
    <data>
        <record id="cansancio_registro_form_view" model="ir.ui.view">
            <field name="name">cansancio.registro.form</field>
            <field name="model">cansancio.registro</field>
            <field name="arch" type="xml">
                <form string="Registro de Cansancio">
                    <sheet>
                        <group>
                            <field name="nombre_alumno"/>
                            <field name="nivel_sueno"/>
                            <field name="bebida_recomendado"/> 
                        </group>
                    </sheet>
                </form>
            </field>
        </record>

        <record id="cansancio_registro_tree_view" model="ir.ui.view">
            <field name="name">cansancio.registro.tree</field>
            <field name="model">cansancio.registro</field>
            <field name="arch" type="xml">
                <list string="Registros de Cansancio">
                    <field name="nombre_alumno"/>
                    <field name="nivel_sueno"/>
                    <field name="bebida_recomendado"/>
                </list>
            </field>
        </record>

        <record id="cansancio_registro_action" model="ir.actions.act_window">
            <field name="name">Calculador de Bebidas</field>
            <field name="res_model">cansancio.registro</field>
            <field name="view_mode">list,form</field>
        </record>

        <menuitem 
            id="cansancio_registro_menu_root"
            name="Control de Sueño"
            sequence="10"
            web_icon="calcular-cansancio,static/description/icon.png"/>

        <menuitem 
            id="cansancio_registro_sub_menu"
            name="Registro de Cansancio"
            parent="cansancio_registro_menu_root"
            action="cansancio_registro_action"
            sequence="1"/>
            
    </data>
</odoo>

## Confirmación del funcionamiento:
Entramos en modo desarollador en Odoo, cambiando la URL anterior a la siguiente:
### http://localhost:8069/odoo/discuss/?debug=1
Vamos a aplicaciones y buscamos nuestra app, en este caso calcular-cansancio y la activamos:
<img width="827" height="403" alt="image" src="https://github.com/user-attachments/assets/dbad8951-b488-40dd-9ad6-08c4e123e1c2" />

<img width="1918" height="538" alt="image" src="https://github.com/user-attachments/assets/331c8e9e-f9ba-486c-8153-257ebe44936a" />








            


            



