\documentclass{article}
\usepackage{hyperref}
\usepackage{listings}
\usepackage{color}

\title{CTF Writeup - \textbf{OverTheWire - Bandit0}}
\author{spiketyson}
\date{14-11-2024}

\begin{document}

\maketitle

\section*{Challenge Details}

\begin{itemize}
    \item \textbf{Challenge Name:} [Challenge Name]
    \item \textbf{Category:} Linux
    \item \textbf{Difficulty Level:} Easy
    \item \textbf{Date:} 14-11-2024
\end{itemize}

\section*{Summary}
This challenge is the introductory level for the Bandit series on OverTheWire, focusing on basic Linux command-line skills. The goal is to connect to a server via SSH and read the file that has the flag.

\section*{Approach}
Since this is an introductory challenge, I planned to use basic Linux commands like \texttt{ssh}, \texttt{ls}, and \texttt{cat} to explore the server and retrieve the flag.

\section*{Solution Walkthrough}

\subsection*{Step 1: Connect via SSH}
Connecting to the server using command \texttt{ssh} as below.

\begin{lstlisting}[language=bash, caption=Connecting to the Server]
ssh bandit0@bandit.labs.overthewire.org -p 2220
# Password: bandit0
\end{lstlisting}

After logging in, I used the \texttt{ls} command to list the files in the directory and found a file named \texttt{readme}.

\begin{lstlisting}[language=bash, caption=Listing Files]
ls
# Output: readme
\end{lstlisting}

Using the \texttt{cat} command, I viewed the contents of \texttt{readme} to extract the flag.

\begin{lstlisting}[language=bash, caption=Extracting the Flag]
cat readme
# Output: FLAG{ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If}
\end{lstlisting}

\section*{Flag}

\textbf{Flag:} [ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If]

\section*{Lessons Learned}
It was a good refresher for using \texttt{ssh} and understanding directory structures on remote servers.

\end{document}
