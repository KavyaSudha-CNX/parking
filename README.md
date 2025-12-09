
--select * from information_schema.columns where column_name ='lob' and table_Schema='master'
--"master_category_primary"  "primary_cat_name"
--"master_category_mapping"  "primary_cat_id"
--v.primary_cat_id,p.primary_cat_id,v.category_id,p.primary_cat_name,s.secondary_cat_name,t.v,
--v.tertiary_cat_id,p.primary_cat_name,s.secondary_cat_name,t.tertiary_cat_name
select v.*,lob.*,rw.*,dt.*,mc.*,t.*,p.*,s.*   from  master.master_category_mapping v
inner join  master.master_category mc on v.category_id=mc.category_id
inner join master.master_lob_channel_casetype_mapping lob on v.lob_channel_casetype_id=lob.id
inner join  master.related_with rw on v.related_with_id=rw.id
inner join master.master_device_type dt on v.device_type_id=dt.device_type_id
inner join master.master_category_tertiary  t  on v.tertiary_cat_id =t.tertiary_cat_id
inner join master.master_category_primary p on v.primary_cat_id=p.primary_cat_id
inner join master.master_category_secondary s on v.secondary_cat_id=s.secondary_cat_id 
order by 1 asc


INBOUND-CRM-ADMIN
INBOUND-CRM-ECALL-SUPERVISOR
INBOUND-CRM-CM-SUPERVISOR
INBOUND-CRM-COMPALINT-MANAGEMENT-SUPERVISOR
INBOUND-CRM-MOS-TEAM-LEAD
INBOUND-CRM-EMAIL-SUPERVISOR
INBOUND-CRM-INBOUND-SUPERVISOR
INBOUND-CRM-SUPERVISOR-TQ
 
DO $$
DECLARE
    r record;
BEGIN
    FOR r IN
        SELECT table_name, column_name
        FROM information_schema.columns
        WHERE table_schema = 'public'
          AND data_type LIKE '%char%'
    LOOP
        EXECUTE format(
            'SELECT ''%s'' AS table_name, ''%s'' AS column_name FROM %I.%I WHERE %I ILIKE ''%%concentrix%%'' LIMIT 1',
            r.table_name, r.column_name, 'public', r.table_name, r.column_name
        );
    END LOOP;
END$$;
