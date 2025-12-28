from crewai import Agent, Task, Crew
import os

# Set your API key (use Grok, OpenAI, Claude, etc.)
os.environ["OPENAI_API_KEY"] = "your-key"

# Tools (example: web search + yfinance)
from langchain.tools import DuckDuckGoSearchRun, YahooFinanceNewsTool

search_tool = DuckDuckGoSearchRun()
finance_tool = YahooFinanceNewsTool()

# Agents
researcher = Agent(
    role='Market Researcher',
    goal='Gather latest global cues, Gift Nifty, news',
    backstory='Expert in Indian and global markets',
    tools=[search_tool, finance_tool],
    verbose=True
)

analyst = Agent(
    role='Technical Analyst',
    goal='Analyze levels, bias, generate stock picks',
    backstory='10+ years Nifty options trader',
    verbose=True
)

writer = Agent(
    role='Report Writer',
    goal='Write structured pre-market report in exact format',
    backstory='Professional market commentator',
    verbose=True
)

# Tasks
task1 = Task(
    description="""Fetch and summarize:
    - Gift Nifty level & change
    - US markets close (Dow, Nasdaq, S&P)
    - Asia opening (Nikkei, Hang Seng)
    - Key news/events today
    - Top movers""",
    agent=researcher,
    expected_output="Bullet point summary"
)

task2 = Task(
    description="""Using data, provide:
    1. Market bias & expected opening
    2. Key Nifty/Bank Nifty levels
    3. 3-5 intraday stock picks with entry/SL/target
    4. Risks""",
    agent=analyst,
    expected_output="Structured analysis"
)

task3 = Task(
    description="""Format final report exactly like this template:
Current date: ...
Pre-market details: ...
1. Overall Market Bias: ...
2. Pre-Market Strength/Weakness: ...
... (full structure)""",
    agent=writer,
    expected_output="Complete formatted report"
)

# Crew
crew = Crew(
    agents=[researcher, analyst, writer],
    tasks=[task1, task2, task3],
    verbose=2
)

result = crew.kickoff()
print(result)---
title: Actions library

contentType: overview
---

# Actions library

This section provides information about n8n's Actions.

