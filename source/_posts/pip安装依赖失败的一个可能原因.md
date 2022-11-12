---
title: pip安装依赖失败的一个可能原因
tags: []
id: '17'
categories:
  - - 开发/服务器踩坑记录
date: 2022-02-01 16:55:08
---

今天在使用弹幕自动切片工具的时候安装依赖的过程报错，错误日志已经不可考，我在网上找到了类似的报错日志以供参考。

在出现如下报错日志的时候，可以用

```
pip --version
```

来检查pip的版本，如果pip的版本没有问题，可以试图关闭vpn来解决，我把代理关闭后马上不报错了。

```
Installing packages failed: Installing packages: error occurred. Details…
ERROR: Exception:
Traceback (most recent call last):
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\cli\base_command.py", line 173, in _main
    status = self.run(options, args)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\cli\req_command.py", line 203, in wrapper
    return func(self, options, args)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\commands\install.py", line 316, in run
    reqs, check_supported_wheels=not options.target_dir
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\resolution\resolvelib\resolver.py", line 95, in resolve
    collected.requirements, max_rounds=try_to_avoid_resolution_too_deep
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\resolvelib\resolvers.py", line 472, in resolve
    state = resolution.resolve(requirements, max_rounds=max_rounds)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\resolvelib\resolvers.py", line 341, in resolve
    self._add_to_criteria(self.state.criteria, r, parent=None)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\resolvelib\resolvers.py", line 172, in _add_to_criteria
    if not criterion.candidates:
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\resolvelib\structs.py", line 151, in __bool__
    return bool(self._sequence)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\resolution\resolvelib\found_candidates.py", line 140, in __bool__
    return any(self)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\resolution\resolvelib\found_candidates.py", line 128, in <genexpr>
    return (c for c in iterator if id(c) not in self._incompatible_ids)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\resolution\resolvelib\found_candidates.py", line 29, in _iter_built
    for version, func in infos:
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\resolution\resolvelib\factory.py", line 275, in iter_index_candidate_infos
    hashes=hashes,
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\package_finder.py", line 851, in find_best_candidate
    candidates = self.find_all_candidates(project_name)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\package_finder.py", line 798, in find_all_candidates
    page_candidates = list(page_candidates_it)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\sources.py", line 134, in page_candidates
    yield from self._candidates_from_page(self._link)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\package_finder.py", line 758, in process_project_url
    html_page = self._link_collector.fetch_page(project_url)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\collector.py", line 490, in fetch_page
    return _get_html_page(location, session=self.session)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\collector.py", line 400, in _get_html_page
    resp = _get_html_response(url, session=session)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\index\collector.py", line 132, in _get_html_response
    "Cache-Control": "max-age=0",
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\requests\sessions.py", line 555, in get
    return self.request('GET', url, **kwargs)
  File "d:\bigdataapps\python\lib\site-packages\pip\_internal\network\session.py", line 454, in request
    return super().request(method, url, *args, **kwargs)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\requests\sessions.py", line 542, in request
    resp = self.send(prep, **send_kwargs)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\requests\sessions.py", line 655, in send
    r = adapter.send(request, **kwargs)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\cachecontrol\adapter.py", line 53, in send
    resp = super(CacheControlAdapter, self).send(request, **kw)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\requests\adapters.py", line 449, in send
    timeout=timeout
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\urllib3\connectionpool.py", line 696, in urlopen
    self._prepare_proxy(conn)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\urllib3\connectionpool.py", line 964, in _prepare_proxy
    conn.connect()
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\urllib3\connection.py", line 359, in connect
    conn = self._connect_tls_proxy(hostname, conn)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\urllib3\connection.py", line 506, in _connect_tls_proxy
    ssl_context=ssl_context,
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\urllib3\util\ssl_.py", line 453, in ssl_wrap_socket
    ssl_sock = _ssl_wrap_socket_impl(sock, context, tls_in_tls)
  File "d:\bigdataapps\python\lib\site-packages\pip\_vendor\urllib3\util\ssl_.py", line 495, in _ssl_wrap_socket_impl
    return ssl_context.wrap_socket(sock)
  File "d:\bigdataapps\python\lib\ssl.py", line 407, in wrap_socket
    _context=self, _session=session)
  File "d:\bigdataapps\python\lib\ssl.py", line 773, in __init__
    raise ValueError("check_hostname requires server_hostname")
ValueError: check_hostname requires server_hostname
```