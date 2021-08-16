# pbar : Fancy progress bar for TUI/CLI

## Usage

```shell
pbar [-b <character_block>] [-c <0-7>] [-t <title>] [-s <total_size>] [<input_file>]
```

## Options

`-b` passed which block to repeat. Must contain only one printable character. **Default**: Full block

`-c` pass a number from 0 to 7 denoting the color of the progress bar. 0=Black; 1=Red; 2=Green; 3=Yellow; 4=Blue; 5=Cyan; 6=Magenta; 7=White. **Default**: 2

`-t` pass title to display

`-s` pass the maximum that the input numbers would reach; i.e. the value at which progress would be 100%. **Default**: 100

## Input

Either provide an input file (normal file or pipe) as argument or input is read from stdin. Input file, if any, is deleted by `pbar` on exit. Input is given in the format:

> index status
>
> index status
>
> ..
>
> index status
>
> final display string

*Index* is a number whose ratio with the total_size or maximum determines the progress percentage.

*Status* is any inline string (containing no new line character) to be displayed below the progress bar at the given percentage.

The final display string is a string to be displayed after progress is complete.

## Example

1. Pipe based

```shell
for i in $(seq 1 10);do
  echo "$i Step: $i"
  ((i!=10)) && sleep 1 || echo Done
done | pbar -s 10 -c 4 -t "Progress Bar"
```

2. Command substitution based

```shell
pbar -s 5 <(for i in `seq 1 5`; do echo $i; sleep 1; done)
```

3. File based. The file is deleted by `pbar` at the end.

```shell
pbar -b '#' /tmp/input &
{ for i in `seq 1 100`; do echo $i; sleep 0.05; done; echo Done;} > /tmp/input
```

