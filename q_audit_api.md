The Audit API allows you to review activity performed by Q. Here are some examples of what you can do with `q.audit` to analyze a mortgage loan after it has closed:

## Review a borrower income calculation on a mortgage

```
q.audit.review({
  loan: 'd82h99bb',
  items: 'income'
});
```
<br>
Result
```
annual: 155200
data sources:
  - name: IRS Transcript 2019
    retrieved: 2020-05-17 12:56:21.281
    wages salaries tips: 155200
```

## Review a mortgage closing disclosure window
```
data = q.audit.review({
  loan: 'd82h99bb',
  items: 'closing'
});

notarization_time = data['notarization']['time'];
closing_disclosure_time = data['closing_disclosure']['time'];

calculate_business_days(notarization_time - closing_disclosure_time);
```
<br>
Result
```
10.23
```

## Re-underwrite a mortgage with fresh data
```
q.audit.reunderwrite({
  loan: 'd82h99bb',
  refresh: ['income', 'credit']
});
```
<br>
Result
```
Underwriting Rules: aa92sjw2
Result: Passed
Income:
  Source: Equifax
  Employer: XYZ Industries
  Income: 155200
  Status: Employed
Credit:
  Source: CoreLogic Trimerge Report
  FICO: [750, 744, 754]
```