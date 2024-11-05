https://github.com/shmilylty/OneForAll

python3 oneforall.py --target ym.all95.cn  --show True run


 """
    OneForAll export from database module

    Example:
        python3 export.py --target name --fmt csv --dir= ./result.csv
        python3 export.py --target name --tb True --show False
        python3 export.py --db result.db --target name --show False

    Note:
        --fmt csv/json (result format)
        --path   Result directory (default directory is ./results)

    :param str  target:  Table to be exported
    :param str  db:      Database path to be exported (default ./results/result.sqlite3)
    :param bool alive:   Only export the results of alive subdomains (default False)
    :param str  limit:   Export limit (default None)
    :param str  fmt:     Result format (default csv)
    :param str  path:    Result directory (default None)
    :param bool show:    Displays the exported data in terminal (default False)
    """



