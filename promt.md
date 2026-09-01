Apply the change: REPLACE the existing onOrderChange prop on CovenantsSection in frontend/src/app/customer/edit/page.tsx:

REMOVE:
    onOrderChange={(nameKey, v) => setCovOrderMap(prev => ({ ...prev, [nameKey]: v }))}

REPLACE WITH:
    onOrderChange={(nameKey, v) => {
      if (v !== 0) {
        const conflict = Object.entries(covOrderMap).some(
          ([n, ord]) => n !== nameKey && Number(ord) === v
        );
        if (conflict) {
          try {
            toast.showWarning(
              `Please only select one Order value for BlackBook display. '${v}' has been selected more than once. To update, first change the existing value to '0'.`
            );
          } catch {}
          return;
        }
      }
      setCovOrderMap(prev => ({ ...prev, [nameKey]: v }));
    }}

Ensure the OLD single-line onOrderChange is REMOVED (only ONE onOrderChange prop remains). Apply now, then confirm there's exactly one onOrderChange on CovenantsSection.
