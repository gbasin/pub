The Audit API allows you to review activity performed by Q. Here are some examples of what you can do with `q.audit` to analyze a mortgage loan after it has closed:

## Review a borrower income calculation on a mortgage

<pre style="background-color: lightgray;"  >
q.audit.<span style="color:red">review</span>({
  loan: 'd82h99bb',
  items: <span style="color:red">'income'</span>
});
</pre>
<br>
Result
<pre style="background-color: lightgray;"  >
annual: 155200
data sources:
  - name: IRS Transcript 2019
    retrieved: 2020-05-17 12:56:21.281
    wages salaries tips: 155200
</pre>

## Review a mortgage closing disclosure window
<pre style="background-color: lightgray;"  >
data = q.audit.<span style="color:red">review</span>({
  loan: 'd82h99bb',
  items: <span style="color:red">'closing'</span>
});

notarization_time = data['notarization']['time'];
closing_disclosure_time = data['closing_disclosure']['time'];

calculate_business_days(notarization_time - closing_disclosure_time);
</pre>
<br>
Result
<pre style="background-color: lightgray;"  >
10.23
</pre>

## Re-underwrite a mortgage with fresh data
<pre style="background-color: lightgray;"  >
q.audit.<span style="color:red">reunderwrite</span>({
  loan: 'd82h99bb',
  items: <span style="color:red">['income', 'credit']</span>
});
</pre>
<br>
Result
<pre style="background-color: lightgray;"  >
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
</pre>