# Mixture-of-Experts-LM-Heads_for_Configurable-Product-Recommendation

## Results on laptop dataset

### laptop dataset

|method|Processor|Graphic Card|Hard Disk|RAM|Screen|
|---|---|---|---|---|---|
|Single Task|47.1|55.7|48.0|45.8|62.4|
|MT-DNN|47.8|59.3|47.4|51.7|62.5|
|LACO|46.6|57.7|48.1|52.0|62.7|
|MTOP|47.4|57.3|47.8|52.5|60.7|
|ModernBERT|48.9|58.4|49.0|**55.1**|63.9|
|MoELMH|**50.1**|**61.7**|**50.1**|53.2|**66.9**|

## Results for extended experiments

### Car seat dataset

|method|Seat Type|Weight Range|Installation Type|Harness Type|
|---|---|---|---|---|
|Single Task|40.5|57.1|64.9|65.1|
|MT-DNN|41.9|59.8|67.3|67.0|
|LACO|41.7|60.5|67.5|67.0|
|MTOP|40.6|60.3|67.0|67.1|
|ModernBERT|42.2|58.6|67.9|67.1|
|MoELMH|**42.7**|**61.1**|**68.0**|**68.2**|

### bike dataset

|method|Bike Type| Age Range| Wheel Size| Number of Speeds| Brake Style| Frame Material| Suspension Type
|---|---|---|---|---|---|---|---|
|Single Task|52.9|74.5|61.7|57.6|54.8|67.7|49.5|
|MT-DNN|56.8|76.1|62.3|58.4|53.1|67.6|49.7|
|LACO|57.1|76.1|61.8|57.8|54.3|67.9|**50.7**|
|MTOP|55.8|75.7|61.3|59.5|55.0|65.4|47.7|
|ModernBERT|55.9|76.3|62.1|58.1|54.1|**68.1**|49.1
|MoELMH|**58.0**|**77.0**|**64.1**|**60.4**|**55.6**|67.6|50.1|

### smartwatch dataset

|method|Screen Size| Display Type| Battery Life
|---|---|---|---|
|Single Task|43.9|45.7|62.5|
|MT-DNN|44.4|45.9|63.7|
|LACO|44.2|44.9|64.6|
|MTOP|43.3|45.4|65.2|
|ModernBERT|44.1|44.9|64.9|
|MoELMH|**45.8**|**47.0**|**67.2**|
