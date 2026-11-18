# SQL: 3 intentos (calificar sin guardar + guardar cuando acepta)

Pegá esto en Supabase → SQL Editor → Run:

```sql
drop function if exists public.calificar(text, text, int[], text);

-- Calificar: solo calcula la nota, NO guarda (vista previa)
create or replace function public.calificar(
  p_examen text,
  p_respuestas int[]
)
returns table (nota int, total int, pct int)
language plpgsql
security definer
set search_path = public
as $$
declare
  v_nota int := 0;
  v_total int;
  v_pct int;
begin
  select count(*) into v_total from public.claves where examen = p_examen;
  select count(*) into v_nota
  from public.claves c
  where c.examen = p_examen
    and c.correcta = p_respuestas[c.num];
  v_pct := round((v_nota::numeric / nullif(v_total, 0)) * 100);
  return query select v_nota, v_total, v_pct;
end;
$$;

-- Guardar: recalcula la nota y la guarda (cuando el alumno está satisfecho)
create or replace function public.guardar_resultado(
  p_nombre text,
  p_examen text,
  p_respuestas int[],
  p_abierta text default null
)
returns table (nota int, total int, pct int)
language plpgsql
security definer
set search_path = public
as $$
declare
  v_nota int := 0;
  v_total int;
  v_pct int;
begin
  select count(*) into v_total from public.claves where examen = p_examen;
  select count(*) into v_nota
  from public.claves c
  where c.examen = p_examen
    and c.correcta = p_respuestas[c.num];
  v_pct := round((v_nota::numeric / nullif(v_total, 0)) * 100);
  insert into public.resultados (nombre, examen, nota, total, pct, respuesta_abierta)
  values (p_nombre, p_examen, v_nota, v_total, v_pct, p_abierta);
  return query select v_nota, v_total, v_pct;
end;
$$;

grant execute on function public.calificar(text, int[]) to anon, authenticated;
grant execute on function public.guardar_resultado(text, text, int[], text) to anon, authenticated;
```
