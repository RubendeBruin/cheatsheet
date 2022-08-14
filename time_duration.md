Measure the time between two events in ms:

```python
start_time = datetime.datetime.now()
        
self.update_outlines()

end_time = datetime.datetime.now()
time_diff = (end_time - start_time)
logging.info(f"... done in {time_diff.total_seconds() * 1000} ms")
```
