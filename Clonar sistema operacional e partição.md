# Passo a Passo para Clonagem de Disco e Atualização de UUIDs no fstab

## Passo 1: Listar Discos e Partições

Use o comando `lsblk` para listar os discos e partições do sistema.

```bash
# lsblk
```

## Passo 2: Clonar o Disco
### Método 1: Mais Lento e Menor Risco de Erros
#### Use o comando dd com blocos de 64K para clonar o disco /dev/sda para o disco /dev/sdb. Este método é mais lento, mas possui menor risco de erros.
```
# dd if=/dev/sda of=/dev/sdb bs=64K status=progress conv=noerror,sync
```

### Método 2: Mais Rápido, Porém Requer Discos em Perfeito Estado
#### Use o comando dd com blocos de 4M para clonar o disco /dev/sda para o disco /dev/sdb. Este método é mais rápido, mas requer que os discos estejam em perfeito estado.
```
# dd if=/dev/sda of=/dev/sdb bs=4M status=progress
```

## Passo 3: Obter os Novos UUIDs
### Após a clonagem, obtenha os novos UUIDs das partições clonadas usando o comando blkid.
```
# blkid
```
#### O comando acima deve ter uma saída esperada do comando blkid parecida com essa abaixo.
```
/dev/sdb1: UUID="1e4de171-2d92-4455-97ad-8c59d0abb00c" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="2c5f772b-01"
/dev/sdb5: UUID="6bd64719-a7df-4fed-9e76-ea8c791cf766" TYPE="swap" PARTUUID="2c5f772b-05"
```

## Passo 4: Editar o Arquivo /mnt/etc/fstab é abrir o Arquivo fstab.
### Abra o arquivo /mnt/etc/fstab com um editor de texto, como o nano.
```
# nano /mnt/etc/fstab
```
#### Substituir os UUIDs
##### Substitua os UUIDs antigos pelos novos UUIDs obtidos do comando blkid.
##### Exemplo abaixo: Altere o UUID pelo obtidido anteriormente se atentando as partições as quais os UUID pertencem.
```
UUID=1e4de171-2d92-4455-97ad-8c59d0abb00c /               ext4    errors=remount-ro 0       1
UUID=6bd64719-a7df-4fed-9e76-ea8c791cf766 none            swap    sw              0       0
```
##### Salve e agora o novo disco está com o SO do antigo e sem perdas, agora é só bootar o SO com ele.



# Conclusão
## Após seguir esses passos, você terá clonado seu disco e atualizado os UUIDs no arquivo fstab para refletir os novos UUIDs das partições clonadas.
