Hi Geoff, quick check for the employee upload: are all FHN Employee IDs
at most 5 digits? Our padding and validation assume 5. If longer IDs
exist, we'll adjust the rule before building it.
