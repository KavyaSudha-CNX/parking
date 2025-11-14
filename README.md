 
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
