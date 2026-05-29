# Velocity

Plan Pareja · presupuesto base cero · A&R

## Despliegue

Esto es un sitio estático. Funciona en cualquier hosting de archivos estáticos. Recomendado: **Render** (gratis).

### Render

1. Hacer fork/upload de este repo a GitHub
2. En render.com → New + → Static Site
3. Conectar el repo
4. Build command: dejar vacío
5. Publish directory: `.`
6. Create Static Site

URL queda tipo `https://velocity-xyz.onrender.com`. En iPhone: Compartir → Añadir a pantalla de inicio.

## Backend

Supabase (PostgreSQL). Una sola tabla:

```sql
create table if not exists velocity_state (
  id integer primary key default 1,
  state jsonb not null default '{}'::jsonb,
  updated_at timestamptz default now(),
  constraint single_row check (id = 1)
);

insert into velocity_state (id, state) values (1, '{}'::jsonb)
  on conflict (id) do nothing;

alter table velocity_state disable row level security;
alter publication supabase_realtime add table velocity_state;
```

## Acceso

PIN compartido configurado en el HTML. Se cachea en el navegador después de la primera vez.

Cerrar sesión: botón 🔒 en el header.
