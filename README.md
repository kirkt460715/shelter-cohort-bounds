# 临时安置需求界限与成组床位受理

这些演练台账是本地合成资料，使用虚构编号。CSV 采用 UTF-8 和带表头的逗号分隔格式。同一单元内用竖线列出的值表示多项或候选集合，含义见各表说明。空值通常表示未记录或不适用，明确的零会保留为 0。

## 资料

- `data/roster.csv`  意向收集条目。cohort 是名册上的同行关系，不自动表示两册同名人同户。assist 为需要无障碍安排的人数。intend 可以是 yes、no 或 unknown。
  字段为 `entry / grid / source / cohort / members / assist / carer / intend`。
- `data/links.csv`  已授权共享的人工核对。choice 相同的行是互斥的关联候选，confirmed 表示已确认的关系。与本表无关的个人字段不存在。
  字段为 `left / right / choice / strength / effective`。
- `data/census.csv`  尚在联系的重点群体和缺席住户。unknown_low 到 unknown_high 是仍可能需要安置的人数范围，不含 roster 上已经定位的住户。
  字段为 `grid / unknown_low / unknown_high / assisted_low / assisted_high / away`。
- `data/shelter.csv`  床位、无障碍床位和照护使用同一登记窗口。pool 表示与其他点共用照护班，care 给该班总名额。
  字段为 `site / window / beds / accessible / care / pool`。
- `data/routes.csv`  接驳可达状态。available=false 时当期路线不能用于安全转移。vehicle 相同且时段重叠的路线共享同一容量。
  字段为 `grid / site / window / vehicle / seats / accessible / available / record_day`。
- `data/protections.csv`  留床保障及当前受理登记。reserved 为空时以列中的重点保障量为准，accepted 时关联 roster 的同行组。改办只接受失去安全到达或照护的已同意记录。
  字段为 `grid / site / window / priority / places / status / reserved / day`。

## 核对台账

环境为 Python 3.11.2，只使用标准库。在仓库根目录运行下列命令检查文件格式、表头和基础外键。输出为各张源表可读行数。

```sh
python3 input_audit.py
python3 -m unittest discover -v
```

可在第一条命令后指定另一个输入目录，目录中的同名 CSV 沿用上述格式。列名和基础引用清单见 `input_contract.json`。
